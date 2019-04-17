---
title: Contenitori di connettersi a una rete virtuale
description: Questo argomento fa parte della Guida alla rete definita dal Software su come gestire carichi di lavoro Tenant e reti virtuali in Windows Server 2016.
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
ms.openlocfilehash: 801cf4b8f71935eb72d820d47e523a310fa64562
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>Connettere gli endpoint del contenitore a una rete virtuale del tenant

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento Mostra come connettere gli endpoint del contenitore a una rete virtuale del tenant esistenti creata attraverso lo stack di Microsoft di rete SDN (Software Defined). Useremo il *l2bridge* (ed eventualmente *l2tunnel*) driver di rete disponibili con il plug-in di Windows libnetwork per Docker di creare una rete contenitore nella macchina virtuale (tenant) host contenitore.

Come descritto nel [rete contenitore](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/management/container_networking) argomento in MSDN, più driver di rete sono disponibili tramite Docker in Windows. I driver più adatti per SDN sono *l2bridge* e *l2tunnel *. Per entrambi i driver, ogni endpoint del contenitore è nella stessa subnet virtuale della macchina virtuale di contenitore host (tenant). Gli indirizzi IP per gli endpoint del contenitore vengono assegnati in modo dinamico per l'Host di rete servizio (SNPP) tramite il plug-in di cloud privato. Gli endpoint del contenitore dispongono di indirizzi IP univoci ma condividono lo stesso indirizzo MAC della macchina virtuale (tenant) host contenitore a causa di conversione dell'indirizzo di livello 2. Criteri di rete (ad esempio: incapsulamento, gli ACL e QoS) per questi endpoint del contenitore vengono imposte nell'host fisico Hyper-V come ricevute dal Controller di rete e definiti in sistemi di gestione di livello superiore. Esiste una leggera differenza tra il *l2bridge* e *l2tunnel* driver descritto di seguito.

- **Bridge L2** -gli endpoint del contenitore che si trovano nella stessa macchina virtuale host contenitore e sono nella stessa subnet avere tutto il traffico di rete con bridging all'interno del commutatore virtuale Hyper-V. Gli endpoint del contenitore che si trovano in diversi host contenitore macchine virtuali o che sono in subnet diverse hanno il loro traffico inoltrato all'host fisico Hyper-V. Poiché il traffico di rete tra i contenitori nello stesso host e nella stessa subnet non vengono trasmessi all'host fisico, non viene applicato alcun criterio di rete. Criterio viene applicato solo per il traffico di rete tra host o tra subnet del contenitore.  
 
- **Tunnel L2** - *tutti* viene inoltrato il traffico di rete tra due endpoint contenitore all'host fisico Hyper-V indipendentemente dall'host o subnet. Criteri di rete viene applicato per il traffico di rete tra subnet e cross-host.   

>[!NOTE]
>Tali modalità di rete non funzionano per gli endpoint del contenitore windows connessione a una rete virtuale tenant in Azure cloud pubblico

## <a name="prerequistes"></a>Prerequisiti
 * È stata distribuita un'infrastruttura SDN con il Controller di rete
 * È stata creata una rete virtuale del tenant
 * Una macchina virtuale tenant è stata distribuita con la funzionalità contenitore di Windows abilitata, Docker installato e funzionalità di Hyper-V abilitato

>[!Note]
>[La virtualizzazione nidificata](https://msdn.microsoft.com/en-us/virtualization/hyperv_on_windows/user_guide/nesting) e l'esposizione di estensioni di virtualizzazione non è obbligatorio a meno che non è necessario per installare i file binari diverse per reti l2bridge e l2tunnel utilizzare funzionalità HyperV di contenitori di Hyper-V

```powershell
# To install HyperV feature without checks for nested virtualization
dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
```

 

## <a name="workflow"></a>Flusso di lavoro

1. [Aggiungere più configurazioni IP a una risorsa VM NIC esistente tramite Controller di rete](#Add) (Host Hyper-V)
2. [Abilitare il proxy di rete nell'host per allocare indirizzi IP CA per gli endpoint del contenitore](#Enable) (Host Hyper-V) 
3. [Installare il plug-in per assegnare gli indirizzi IP CA per gli endpoint del contenitore del cloud privato](#Install) (macchina virtuale Host contenitore) 
4. [Creare un *l2bridge* o *l2tunnel* rete usando docker](#Create) (macchina virtuale Host contenitore) 
 
>[!NOTE]
>Più configurazioni di IP non è supportato in risorse NIC VM create tramite System Center Virtual Machine Manager. È consigliabile per questi tipi di distribuzioni che si crea la risorsa VM NIC fuori banda utilizzando PowerShell Controller di rete.

### <a name="Add"></a>1. aggiungere più configurazioni IP

Per questo esempio, partiamo dal presupposto che la scheda NIC VM della macchina virtuale tenant è una configurazione IP con indirizzo IP del 192.168.1.9 già ed è collegata a un ID di risorsa di rete virtuale di 'VNet1' e delle risorse di Subnet VM di subnet '1' nella subnet IP 192.168.1.0/24. Verrà aggiunto da 192.168.1.101 - 192.168.1.110 10 indirizzi IP per i contenitori.

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

### <a name="Enable"></a>2. abilitare il Proxy di rete

[ConfigureMCNP.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1>)

Eseguire lo script al **Host Hyper-V** che ospita la macchina virtuale di contenitore host (tenant) per abilitare il proxy di rete allocare più indirizzi IP per la macchina virtuale host contenitore.

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="Install"></a>3. installare plug-in Cloud privato

[InstallPrivateCloudPlugin.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1)

Eseguire lo script all'interno di **macchina virtuale host (tenant) di contenitore** per consentire il servizio di rete Host (SNPP) per comunicare con il proxy di rete nell'Host Hyper-V.

```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="Create"></a>4. creare un *l2bridge* rete contenitore

Nel **macchina virtuale host (tenant) di contenitore** utilizzare il `docker network create`comando per creare una rete l2bridge

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>Assegnazione di IP statico non è supportato con *l2bridge* o *l2tunnel* contenitore reti quando si utilizza lo Stack di SDN Microsoft.

## <a name="more-information"></a>Altre informazioni
Per ulteriori infortation sulla distribuzione di un'infrastruttura SDN, vedere [distribuire un'infrastruttura di rete Software definito](https://technet.microsoft.com/en-us/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure).

