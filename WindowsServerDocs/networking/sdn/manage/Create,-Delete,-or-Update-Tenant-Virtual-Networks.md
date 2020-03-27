---
title: Creare, eliminare o aggiornare la rete virtuale del tenant
description: In questo argomento viene illustrato come creare, eliminare e aggiornare le reti virtuali di virtualizzazione rete Hyper-V dopo la distribuzione di SDN (Software Defined Networking). Virtualizzazione rete Hyper-V consente di isolare le reti tenant in modo che ogni rete tenant sia un'entità separata. Ogni entità non ha alcuna possibilità di connessione incrociata a meno che non vengano configurati carichi di lavoro di accesso pubblico.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a820826-e829-4ef2-9a20-f74235f8c25b
ms.author: lizross
author: eross-msft
ms.date: 08/24/2018
ms.openlocfilehash: f85f593ec3dca33c5b35fb065c7d84ed12ea9af2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309828"
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>Creare, eliminare o aggiornare le reti virtuali tenant

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento viene illustrato come creare, eliminare e aggiornare le reti virtuali di virtualizzazione rete Hyper-V dopo la distribuzione di SDN (Software Defined Networking). Virtualizzazione rete Hyper-V consente di isolare le reti tenant in modo che ogni rete tenant sia un'entità separata. Ogni entità non ha alcuna possibilità di connessione incrociata a meno che non vengano configurati carichi di lavoro di accesso pubblico.   
  
## <a name="create-a-new-virtual-network"></a>Creare una nuova rete virtuale  
La creazione di una rete virtuale per un tenant lo inserisce in un dominio di routing univoco nell'host Hyper-V. Al di sotto di ogni rete virtuale è presente almeno una subnet virtuale. Le subnet virtuali vengono definite da un prefisso IP e fanno riferimento a un ACL definito in precedenza.  

I passaggi per creare una nuova rete virtuale sono:

1. Identificare i prefissi degli indirizzi IP da cui si desidera creare le subnet virtuali.   
2. Identificare la rete di provider logici su cui il traffico tenant viene sottoposto a tunneling.   
3. Creare almeno una subnet virtuale per ogni prefisso IP identificato nel passaggio 1. 
4. Opzionale Aggiungere gli ACL creati in precedenza alle subnet virtuali o aggiungere la connettività del gateway per i tenant. 

La tabella seguente include gli ID subnet di esempio e i prefissi per due tenant fittizi. Il tenant Fabrikam ha due subnet virtuali, mentre il tenant Contoso ha tre subnet virtuali.  
 
  
Nome tenant  |ID subnet virtuale  |Prefisso subnet virtuale    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
Lo script di esempio seguente usa i comandi di Windows PowerShell esportati dal modulo **NetworkController** per creare la rete virtuale di Contoso e una subnet:   
  
```Powershell  
import-module networkcontroller  
$URI = "https://ncrest.contoso.local"  
  
#Find the HNV Provider Logical Network  
  
$logicalnetworks = Get-NetworkControllerLogicalNetwork -ConnectionUri $uri  
foreach ($ln in $logicalnetworks) {  
   if ($ln.Properties.NetworkVirtualizationEnabled -eq "True") {  
      $HNVProviderLogicalNetwork = $ln  
   }  
}   
  
#Find the Access Control List to user per virtual subnet  
  
$acllist = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"  
  
#Create the Virtual Subnet  
  
$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso_WebTier"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AccessControlList = $acllist  
$vsubnet.Properties.AddressPrefix = "24.30.1.0/24"  
  
#Create the Virtual Network  
  
$vnetproperties = new-object Microsoft.Windows.NetworkController.VirtualNetworkProperties  
$vnetproperties.AddressSpace = new-object Microsoft.Windows.NetworkController.AddressSpace  
$vnetproperties.AddressSpace.AddressPrefixes = @("24.30.1.0/24")  
$vnetproperties.LogicalNetwork = $HNVProviderLogicalNetwork  
$vnetproperties.Subnets = @($vsubnet)  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -Properties $vnetproperties  
  
```  
  
## <a name="modify-an-existing-virtual-network"></a>Modificare una rete virtuale esistente  
È possibile utilizzare Windows PowerShell per aggiornare una subnet o una rete virtuale esistente.   
  
Quando si esegue lo script di esempio seguente, le risorse aggiornate vengono semplicemente inserite nel controller di rete con lo stesso ID di risorsa. Se il tenant Contoso desidera aggiungere una nuova subnet virtuale (24.30.2.0/24) alla rete virtuale, l'utente o l'amministratore di Contoso può utilizzare lo script seguente.  
  
```PowerShell  
$acllist = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"  
  
$vnet = Get-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri  
  
$vnet.properties.AddressSpace.AddressPrefixes += "24.30.2.0/24"  
  
$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso_DBTier"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AccessControlList = $acllist  
$vsubnet.Properties.AddressPrefix = "24.30.2.0/24"  
  
$vnet.properties.Subnets += $vsubnet  
  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -properties $vnet.properties  
  
```  
  
## <a name="delete-a-virtual-network"></a>Eliminare una rete virtuale  
  
È possibile utilizzare Windows PowerShell per eliminare una rete virtuale.  
  
Nell'esempio di Windows PowerShell seguente viene eliminata una rete virtuale tenant emettendo un'operazione HTTP DELETE nell'URI dell'ID risorsa.  

```PowerShell  
Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  
```

