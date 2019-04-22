---
title: Configura il peering della rete virtuale
description: Configurare il peering di rete virtuale consiste nel creare due reti virtuali di cui ottengano il peering.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: 3ef3db879080e3372e7b287dcc55ae052c1fe109
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816392"
---
# <a name="configure-virtual-network-peering"></a>Configura il peering della rete virtuale

>Si applica a: Windows Server

In questa procedura usare Windows PowerShell per creare due reti virtuali, ognuna con una subnet. Quindi, si configurare il peering tra le due reti virtuali per abilitare la connettività tra di essi.

- [Passaggio 1. Creare la prima rete virtuale](#step-1-create-the-first-virtual-network)

- [Passaggio 2. Creare la seconda rete virtuale](#step-2-create-the-second-virtual-network)

- [Passaggio 3. Configurare il peering dalla prima rete virtuale alla seconda rete virtuale](#step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network)

- [Passaggio 4. Configurare il peering dalla seconda rete virtuale per la prima rete virtuale](#step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network)


>[!IMPORTANT]
>Ricordarsi di aggiornare le proprietà per l'ambiente.

## <a name="step-1-create-the-first-virtual-network"></a>Passaggio 1. Creare la prima rete virtuale

In questo passaggio si usa ricerca di Windows PowerShell la rete logica provider HNV per creare la prima rete virtuale con una subnet. Lo script di esempio seguente crea rete virtuale di Contoso con una subnet.

``` PowerShell
#Find the HNV Provider Logical Network  

$logicalnetworks = Get-NetworkControllerLogicalNetwork -ConnectionUri $uri  
foreach ($ln in $logicalnetworks) {  
   if ($ln.Properties.NetworkVirtualizationEnabled -eq "True") {  
      $HNVProviderLogicalNetwork = $ln  
   }  
}   

#Create the Virtual Subnet  

$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AddressPrefix = "24.30.1.0/24"
$uri=”https://restserver”  

#Create the Virtual Network  

$vnetproperties = new-object Microsoft.Windows.NetworkController.VirtualNetworkProperties  
$vnetproperties.AddressSpace = new-object Microsoft.Windows.NetworkController.AddressSpace  
$vnetproperties.AddressSpace.AddressPrefixes = @("24.30.1.0/24")  
$vnetproperties.LogicalNetwork = $HNVProviderLogicalNetwork  
$vnetproperties.Subnets = @($vsubnet)  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -Properties $vnetproperties
```

## <a name="step-2-create-the-second-virtual-network"></a>Passaggio 2. Creare la seconda rete virtuale

In questo passaggio si crea una seconda rete virtuale con una subnet. Lo script di esempio seguente crea rete virtuale di Woodgrove con una subnet.

``` PowerShell

#Create the Virtual Subnet  

$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Woodgrove"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AddressPrefix = "24.30.2.0/24"  
$uri=”https://restserver”

#Create the Virtual Network  

$vnetproperties = new-object Microsoft.Windows.NetworkController.VirtualNetworkProperties  
$vnetproperties.AddressSpace = new-object Microsoft.Windows.NetworkController.AddressSpace  
$vnetproperties.AddressSpace.AddressPrefixes = @("24.30.2.0/24")  
$vnetproperties.LogicalNetwork = $HNVProviderLogicalNetwork  
$vnetproperties.Subnets = @($vsubnet)  
New-NetworkControllerVirtualNetwork -ResourceId "Woodgrove_VNet1" -ConnectionUri $uri -Properties $vnetproperties
```

## <a name="step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network"></a>Passaggio 3. Configurare il peering dalla prima rete virtuale alla seconda rete virtuale

In questo passaggio si configura il peering tra la prima rete virtuale e la seconda rete virtuale creata in due passaggi precedenti. Lo script di esempio seguente stabilisce il peering di rete virtuale dalla **Contoso_vnet1** al **Woodgrove_vnet1**.

```PowerShell
$peeringProperties = New-Object Microsoft.Windows.NetworkController.VirtualNetworkPeeringProperties
$vnet2 = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Woodgrove_VNet1"
$peeringProperties.remoteVirtualNetwork = $vnet2

#Indicate whether communication between the two virtual networks
$peeringProperties.allowVirtualnetworkAccess = $true

#Indicates whether forwarded traffic is allowed across the vnets
$peeringProperties.allowForwardedTraffic = $true

#Indicates whether the peer virtual network can access this virtual networks gateway
$peeringProperties.allowGatewayTransit = $false

#Indicates whether this virtual network uses peer virtual networks gateway
$peeringProperties.useRemoteGateways =$false

New-NetworkControllerVirtualNetworkPeering -ConnectionUri $uri -VirtualNetworkId “Contoso_vnet1” -ResourceId “ContosotoWoodgrove” -Properties $peeringProperties

```

>[!IMPORTANT]
>Dopo aver creato questo peering, viene illustrato lo stato della rete virtuale **Initiated**.

## <a name="step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network"></a>Passaggio 4. Configurare il peering dalla seconda rete virtuale per la prima rete virtuale

In questo passaggio si configura il peering tra la seconda rete virtuale e la prima rete virtuale creata nei passaggi 1 e 2 descritti sopra. Lo script di esempio seguente stabilisce il peering di rete virtuale dalla **Woodgrove_vnet1** al **Contoso_vnet1**.

```PowerShell
$peeringProperties = New-Object Microsoft.Windows.NetworkController.VirtualNetworkPeeringProperties 
$vnet2=Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Contoso_VNet1"
$peeringProperties.remoteVirtualNetwork = $vnet2 

# Indicates whether communication between the two virtual networks is allowed 
$peeringProperties.allowVirtualnetworkAccess = $true 

# Indicates whether forwarded traffic will be allowed across the vnets
$peeringProperties.allowForwardedTraffic = $true 

# Indicates whether the peer virtual network can access this virtual network’s gateway
$peeringProperties.allowGatewayTransit = $false 

# Indicates whether this virtual network will use peer virtual network’s gateway
$peeringProperties.useRemoteGateways =$false 

New-NetworkControllerVirtualNetworkPeering -ConnectionUri $uri -VirtualNetworkId “Woodgrove_vnet1” -ResourceId “WoodgrovetoContoso” -Properties $peeringProperties 

```

Dopo aver creato questo peering, il stato di peering reti virtuali illustra **Connected** per entrambi i peer. A questo punto, le macchine virtuali in una rete virtuale possono comunicare con le macchine virtuali nella rete virtuale con peering.

---