---
title: Connettere gli endpoint del contenitore a una rete virtuale del tenant
description: In questo argomento viene illustrato come connettere gli endpoint del contenitore a una rete virtuale tenant esistente creata tramite SDN. Usare il driver di rete l2bridge (e facoltativamente l2tunnel) disponibile con il plug-in Windows libnetwork per Docker per creare una rete di contenitori nella VM tenant.
manager: ravirao
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f7af1eb6-d035-4f74-a25b-d4b7e4ea9329
ms.author: lizross
author: jmesser81
ms.date: 08/24/2018
ms.openlocfilehash: 5673cb6f808f37fb7737e22cf93c3984073e4f48
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309831"
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>Connettere gli endpoint del contenitore a una rete virtuale del tenant

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento viene illustrato come connettere gli endpoint del contenitore a una rete virtuale tenant esistente creata tramite SDN. Usare il driver di rete *l2bridge* (e facoltativamente *l2tunnel*) disponibile con il plug-in Windows libnetwork per Docker per creare una rete di contenitori nella VM tenant.

Nell'argomento [driver di rete del contenitore](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/network-drivers-topologies) sono stati discussi i driver di rete più disponibili tramite Docker in Windows. Per SDN, usare i driver *l2bridge* e *l2tunnel* . Per entrambi i driver, ogni endpoint del contenitore si trova nella stessa subnet virtuale della macchina virtuale dell'host contenitore (tenant). 

Il servizio di rete host (HNS), tramite il plug-in del cloud privato, assegna in modo dinamico gli indirizzi IP per gli endpoint del contenitore. Gli endpoint del contenitore dispongono di indirizzi IP univoci, ma condividono lo stesso indirizzo MAC della macchina virtuale dell'host contenitore (tenant) a causa della conversione degli indirizzi di livello 2. 

I criteri di rete (ACL, incapsulamento e QoS) per questi endpoint contenitore vengono applicati nell'host Hyper-V fisico come ricevuto dal controller di rete e definiti nei sistemi di gestione di livello superiore. 

La differenza tra i driver *l2bridge* e *l2tunnel* è la seguente:


|                                                                                                                                                                                                                                                                            l2bridge                                                                                                                                                                                                                                                                            |                                                                                                 l2tunnel                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Endpoint contenitore che risiedono in: <ul><li>La stessa macchina virtuale host del contenitore e nella stessa subnet hanno tutto il traffico di rete con collegamento all'interno del Commuter virtuale Hyper-V. </li><li>Le diverse VM host del contenitore o in subnet diverse hanno il traffico trasmesso all'host Hyper-V fisico. </li></ul>I criteri di rete non vengono applicati perché il traffico di rete tra i contenitori nello stesso host e nella stessa subnet non viene propagato all'host fisico. I criteri di rete si applicano solo al traffico di rete del contenitore tra più host o tra subnet. | *Tutto* il traffico di rete tra due endpoint contenitore viene inviato all'host Hyper-V fisico indipendentemente dall'host o dalla subnet. I criteri di rete si applicano al traffico di rete tra subnet e tra host. |

---

>[!NOTE]
>Queste modalità di rete non funzionano per la connessione degli endpoint del contenitore Windows a una rete virtuale tenant nel cloud pubblico di Azure.


## <a name="prerequisites"></a>Prerequisiti
-  Un'infrastruttura SDN distribuita con il controller di rete.
-  È stata creata una rete virtuale tenant.
-  Una macchina virtuale tenant distribuita con la funzionalità contenitore di Windows abilitata, Docker installato e la funzionalità Hyper-V abilitata. La funzionalità Hyper-V è necessaria per installare diversi file binari per le reti l2bridge e l2tunnel.

   ```powershell
   # To install HyperV feature without checks for nested virtualization
   dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
   ```

>[!Note]
>La [virtualizzazione annidata](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/nesting) e l'esposizione delle estensioni di virtualizzazione non sono necessarie a meno che non si utilizzino contenitori Hyper-V 


## <a name="workflow"></a>Flusso di lavoro

[1. aggiungere più configurazioni IP a una risorsa NIC esistente della macchina virtuale tramite il controller di rete (host Hyper-V)](#1-add-multiple-ip-configurations)
[2. Abilitare il proxy di rete nell'host per allocare gli indirizzi IP della CA per gli endpoint del contenitore (host Hyper-V)](#2-enable-the-network-proxy)
[3. Installare il plug-in del cloud privato per assegnare gli indirizzi IP della CA agli endpoint del contenitore (VM host contenitore)](#3-install-the-private-cloud-plug-in)
[4. Creare una rete *l2bridge* o *l2tunnel* usando Docker (VM host contenitore)](#4-create-an-l2bridge-container-network)

>[!NOTE]
>Non sono supportate più configurazioni IP nelle risorse NIC della macchina virtuale create con System Center Virtual Machine Manager. Per questi tipi di distribuzioni è consigliabile creare la risorsa NIC della VM fuori banda usando il controller di rete PowerShell.

### <a name="1-add-multiple-ip-configurations"></a>1. aggiungere più configurazioni IP
In questo passaggio si presuppone che la scheda di interfaccia di rete della VM della macchina virtuale tenant abbia una configurazione IP con indirizzo IP di 192.168.1.9 ed è collegata a un ID risorsa VNet di "VNet1" e alla risorsa della subnet VM di "Subnet1" nella subnet IP 192.168.1.0/24. Si aggiungono 10 indirizzi IP per i contenitori da 192.168.1.101-192.168.1.110.

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

### <a name="2-enable-the-network-proxy"></a>2. abilitare il proxy di rete
In questo passaggio viene abilitato il proxy di rete per l'allocazione di più indirizzi IP per la macchina virtuale host del contenitore. 

Per abilitare il proxy di rete, eseguire lo script [ConfigureMCNP. ps1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1) nell' **host Hyper-V** che ospita la macchina virtuale dell'host contenitore (tenant).

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="3-install-the-private-cloud-plug-in"></a>3. installare il plug-in del cloud privato
In questo passaggio viene installato un plug-in per consentire al HNS di comunicare con il proxy di rete nell'host Hyper-V.

Per installare il plug-in, eseguire lo script [InstallPrivateCloudPlugin. ps1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1) nella **macchina virtuale dell'host contenitore (tenant)** .


```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="4-create-an-l2bridge-container-network"></a>4. creare una rete di contenitori *l2bridge*
In questo passaggio si usa il comando `docker network create` nella **macchina virtuale dell'host contenitore (tenant)** per creare una rete l2bridge. 

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>L'assegnazione di indirizzi IP statici non è supportata con le reti di contenitori *l2bridge* o *l2tunnel* se usata con lo stack Sdn di Microsoft.

## <a name="more-information"></a>Altre informazioni
Per altre informazioni sulla distribuzione di un'infrastruttura SDN, vedere [distribuire un'infrastruttura software defined Network](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure).

