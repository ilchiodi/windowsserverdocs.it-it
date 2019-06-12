---
title: Connettere gli endpoint del contenitore a una rete virtuale del tenant
description: In questo argomento viene illustrato come connettere gli endpoint del contenitore a una rete virtuale tenant esistente creata tramite SDN. Si utilizza il l2bridge (e facoltativamente l2tunnel) driver di rete disponibili con il plug-in di Windows libnetwork per Docker per creare una rete di contenitori nella macchina virtuale tenant.
manager: ravirao
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f7af1eb6-d035-4f74-a25b-d4b7e4ea9329
ms.author: pashort
author: jmesser81
ms.date: 08/24/2018
ms.openlocfilehash: cb9c7157ffb07233e41e1c933f6775f1cd0766a9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446355"
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>Connettere gli endpoint del contenitore a una rete virtuale del tenant

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento viene illustrato come connettere gli endpoint del contenitore a una rete virtuale tenant esistente creata tramite SDN. Si utilizza il *l2bridge* (e facoltativamente *l2tunnel*) driver di rete disponibili con il plug-in di Windows libnetwork per Docker per creare una rete di contenitori nella macchina virtuale tenant.

Nel [i driver di rete di contenitori](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/network-drivers-topologies) argomento discussa più driver di rete sono disponibili tramite Docker in Windows. Per la rete SDN, usare il *l2bridge* e *l2tunnel* driver. Per entrambi i driver, ogni endpoint del contenitore è nella stessa subnet virtuale come macchina virtuale (tenant) di host contenitore. 

L'Host di rete Service (HNS), tramite il plug-in di cloud privato, assegna in modo dinamico gli indirizzi IP per gli endpoint del contenitore. Gli endpoint del contenitore hanno indirizzi IP univoci, ma condividono lo stesso indirizzo MAC della macchina virtuale (tenant) di host contenitore a causa di conversione degli indirizzi di livello 2. 

Criteri di rete (ACL, l'incapsulamento e QoS) per questi endpoint contenitore sono applicati nell'host Hyper-V fisico come ricevuto dal Controller di rete e definiti nei sistemi di gestione di livello superiore. 

La differenza tra il *l2bridge* e *l2tunnel* driver sono:


|                                                                                                                                                                                                                                                                            l2bridge                                                                                                                                                                                                                                                                            |                                                                                                 l2tunnel                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Endpoint del contenitore che si trovano in: <ul><li>Lo stesso contenitore host macchina virtuale e nella stessa subnet ha tutto il traffico di rete con bridging nel commutatore virtuale Hyper-V. </li><li>Contenitore diverso ospitare le macchine virtuali o in subnet diverse hanno il traffico inoltrato all'host Hyper-V fisico. </li></ul>Criteri di rete non ottenere applicato poiché il traffico di rete tra i contenitori nello stesso host e nella stessa subnet non vengono indirizzate all'host fisico. Criteri di rete si applicano il traffico di rete di contenitori solo su cross-host o tra subnet. | *Tutti i* viene inoltrato il traffico di rete tra due endpoint di contenitore nell'host Hyper-V fisici indipendentemente dall'host o la subnet. Criteri di rete si applicano al traffico di rete tra subnet e cross-host. |

---

>[!NOTE]
>Queste modalità di rete non funzionano per gli endpoint del contenitore windows che si connette a una rete virtuale tenant nel cloud pubblico di Azure.


## <a name="prerequisites"></a>Prerequisiti
-  Un'infrastruttura SDN distribuita con il Controller di rete.
-  Una rete virtuale del tenant è stata creata.
-  Una macchina virtuale tenant distribuito con la funzionalità contenitore di Windows abilitato, Docker installato e funzionalità di Hyper-V abilitato. La funzionalità di Hyper-V è necessaria installare i file binari diversi per le reti l2bridge e l2tunnel.

   ```powershell
   # To install HyperV feature without checks for nested virtualization
   dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
   ```

>[!Note]
>[La virtualizzazione annidata](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/nesting) e che espone le estensioni di virtualizzazione non è obbligatorio a meno che non usano i contenitori di Hyper-V. 


## <a name="workflow"></a>Flusso di lavoro

[1. Aggiungere più configurazioni IP a una risorsa NIC VM esistente tramite il Controller di rete (Host Hyper-V)](#1-add-multiple-ip-configurations)
[2. Abilitare il proxy di rete nell'host per allocare indirizzi IP CA per gli endpoint del contenitore (Host Hyper-V)](#2-enable-the-network-proxy)
[3. Installare il plug-in per assegnare gli indirizzi IP CA agli endpoint del contenitore (macchina virtuale Host contenitore) di cloud privato](#3-install-the-private-cloud-plug-in)
[4. Creare un *l2bridge* oppure *l2tunnel* rete usando docker (macchina virtuale Host contenitore)](#4-create-an-l2bridge-container-network)

>[!NOTE]
>Più configurazioni IP non è supportato sulle risorse di VM NIC create tramite System Center Virtual Machine Manager. È consigliabile per questi tipi di distribuzioni che si crea la risorsa VM NIC fuori banda tramite PowerShell di Controller di rete.

### <a name="1-add-multiple-ip-configurations"></a>1. Aggiungere più configurazioni IP
In questo passaggio si presuppone che la VM NIC della macchina virtuale tenant ha una configurazione IP con l'indirizzo IP di 192.168.1.9 e collegata a un ID risorsa della rete virtuale "VNet1" e risorsa VM Subnet "Subnet1" nella subnet IP 192.168.1.0/24. Aggiungiamo 10 indirizzi IP per i contenitori da 192.168.1.101 - 192.168.1.110.

```powershell
Import-Module NetworkController

# Specify Network Controller REST IP or FQDN
$uri = "<NC REST IP or FQDN>"
$vnetResourceId = "VNet1"
$vsubnetResourceId = "Subnet1"

$vmnic= Get-NetworkControllerNetworkInterface -ConnectionUri $uri | where {$_.properties.IpConfigurations.Properties.PrivateIPAddress -eq "192.168.1.9" }
$vmsubnet = Get-NetworkControllerVirtualSubnet -VirtualNetworkId $vnetResourceId -ResourceId $vsubnetResourceId -ConnectionUri $uri

# For this demo, we will assume an ACL has already been defined; any ACL can be applied here
$allowallacl = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"


foreach ($i in 1..10)
{
    $newipconfig = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
    $props = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties

    $resourceid = "IP_192_168_1_1"
    if ($i -eq 10) 
    {
        $resourceid += "10"
        $ipstr = "192.168.1.110"
    }
    else
    {
        $resourceid += "0$i"
        $ipstr = "192.168.1.10$i"
    }

    $newipconfig.ResourceId = $resourceid
    $props.PrivateIPAddress = $ipstr    

    $props.PrivateIPAllocationMethod = "Static"
    $props.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $props.Subnet.ResourceRef = $vmsubnet.ResourceRef
    $props.AccessControlList = new-object Microsoft.Windows.NetworkController.AccessControlList
    $props.AccessControlList.ResourceRef = $allowallacl.ResourceRef

    $newipconfig.Properties = $props
    $vmnic.Properties.IpConfigurations += $newipconfig
}

New-NetworkControllerNetworkInterface -ResourceId $vmnic.ResourceId -Properties $vmnic.Properties -ConnectionUri $uri
```

### <a name="2-enable-the-network-proxy"></a>2. Abilitare il proxy di rete
In questo passaggio si abilita il proxy di rete allocare più indirizzi IP per la macchina virtuale host contenitore. 

Per abilitare il proxy di rete, eseguire la [ConfigureMCNP.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1) script il **Host Hyper-V** ospita la macchina virtuale host (tenant) di contenitore.

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="3-install-the-private-cloud-plug-in"></a>3. Installare il plug-in Cloud privato
In questo passaggio si installa un plug-in per consentire il HNS comunicare con il proxy di rete nell'Host Hyper-V.

Per installare il plug-in, eseguire la [InstallPrivateCloudPlugin.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1) script all'interno di **macchina virtuale host (tenant) del contenitore**.


```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="4-create-an-l2bridge-container-network"></a>4. Creare un *l2bridge* rete di contenitori
In questo passaggio, Usa la `docker network create` comando il **macchina virtuale host (tenant) del contenitore** per creare una rete l2bridge. 

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>Assegnazione IP statico non è supportata con *l2bridge* oppure *l2tunnel* contenitore reti quando usato con lo Stack SDN di Microsoft.

## <a name="more-information"></a>Altre informazioni
Per altre informazioni sulla distribuzione di un'infrastruttura SDN, vedere [distribuire un'infrastruttura Software Defined Network](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure).

