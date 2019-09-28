---
title: Configura il peering della rete virtuale
description: La configurazione del peering di rete virtuale comporta la creazione di due reti virtuali con peering.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: 4d35501b8d876f2a178a4744d495125dea8da6c7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405813"
---
# <a name="configure-virtual-network-peering"></a>Configura il peering della rete virtuale

>Si applica a: Windows Server

In questa procedura viene usato Windows PowerShell per creare due reti virtuali, ognuna con una subnet. Configurare quindi il peering tra le due reti virtuali per consentire la connettività tra di essi.

- [Passaggio 1. Creare la prima rete virtuale](#step-1-create-the-first-virtual-network)

- [Passaggio 2. Creare la seconda rete virtuale](#step-2-create-the-second-virtual-network)

- [Passaggio 3. Configurare il peering dalla prima rete virtuale alla seconda rete virtuale](#step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network)

- [Passaggio 4. Configurare il peering dalla seconda rete virtuale alla prima rete virtuale](#step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network)


>[!IMPORTANT]
>Ricordarsi di aggiornare le proprietà dell'ambiente.

## <a name="step-1-create-the-first-virtual-network"></a>Passaggio 1. Creare la prima rete virtuale

In questo passaggio viene usato Windows PowerShell per trovare la rete logica del provider HNV per creare la prima rete virtuale con una subnet. Lo script di esempio seguente crea la rete virtuale di Contoso con una subnet.

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

In questo passaggio viene creata una seconda rete virtuale con una subnet. Lo script di esempio seguente crea la rete virtuale di Woodgrove con una subnet.

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

In questo passaggio si configura il peering tra la prima rete virtuale e la seconda rete virtuale creata nei due passaggi precedenti. Lo script di esempio seguente stabilisce il peering di rete virtuale da **Contoso_vnet1** a **Woodgrove_vnet1**.

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
>Dopo la creazione di questo peering, lo stato di VNET indica **avviato**.

## <a name="step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network"></a>Passaggio 4. Configurare il peering dalla seconda rete virtuale alla prima rete virtuale

In questo passaggio si configura il peering tra la seconda rete virtuale e la prima rete virtuale creata nei passaggi 1 e 2 precedenti. Lo script di esempio seguente stabilisce il peering di rete virtuale da **Woodgrove_vnet1** a **Contoso_vnet1**.

```PowerShell
$peeringProperties = New-Object Microsoft.Windows.NetworkController.VirtualNetworkPeeringProperties 
$vnet2=Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Contoso_VNet1"
$peeringProperties.remoteVirtualNetwork = $vnet2 

# Indicates whether communication between the two virtual networks is allowed 
$peeringProperties.allowVirtualnetworkAccess = $true 

# Indicates whether forwarded traffic will be allowed across the vnets
$peeringProperties.allowForwardedTraffic = $true 

# Indicates whether the peer virtual network can access this virtual network's gateway
$peeringProperties.allowGatewayTransit = $false 

# Indicates whether this virtual network will use peer virtual network's gateway
$peeringProperties.useRemoteGateways =$false 

New-NetworkControllerVirtualNetworkPeering -ConnectionUri $uri -VirtualNetworkId “Woodgrove_vnet1” -ResourceId “WoodgrovetoContoso” -Properties $peeringProperties 

```

Dopo la creazione di questo peering, lo stato del peering VNET Visualizza **connesso** per entrambi i peer. A questo punto, le macchine virtuali in una rete virtuale possono comunicare con le macchine virtuali nella rete virtuale con peering.

---