---
title: Creare, eliminare o aggiornare la rete virtuale tenant
description: In questo argomento descrive come creare, eliminare e aggiornare le reti virtuali di Hyper-V rete virtualizzazione dopo la distribuzione di rete SDN (Software Defined). Virtualizzazione rete Hyper-V consente di isolare le reti tenant in modo che ogni rete tenant è un'entità distinta. Ogni entità non dispone di alcuna possibilità di cross connection a meno che non si configura l'accesso pubblico dei carichi di lavoro.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a820826-e829-4ef2-9a20-f74235f8c25b
ms.author: pashort
author: shortpatti
ms.date: 08/24/2018
ms.openlocfilehash: a125ec220b4769a57a6be30f1425283afb7f0fe6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838352"
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>Creare, eliminare o aggiornare le reti virtuali tenant

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento descrive come creare, eliminare e aggiornare le reti virtuali di Hyper-V rete virtualizzazione dopo la distribuzione di rete SDN (Software Defined). Virtualizzazione rete Hyper-V consente di isolare le reti tenant in modo che ogni rete tenant è un'entità distinta. Ogni entità non dispone di alcuna possibilità di cross connection a meno che non si configura l'accesso pubblico dei carichi di lavoro.   
  
## <a name="create-a-new-virtual-network"></a>Creare una nuova rete virtuale  
Creazione di una rete virtuale per un tenant per inserirlo all'interno di un dominio di routing univoco nell'host Hyper-V. Di sotto di ogni rete virtuale, è presente almeno una subnet virtuale. Subnet virtuali ottengono definita da un prefisso IP e fare riferimento a un ACL definito in precedenza.  

I passaggi per creare una nuova rete virtuale sono:

1. Identificare i prefissi degli indirizzi IP da cui si desidera creare le subnet virtuali.   
2. Identificare la rete logica provider su cui il traffico tenant è sottoposto a tunneling.   
3. Creare almeno una subnet virtuale per ogni prefisso IP identificato nel passaggio 1. 
4. (Facoltativo) Aggiungere gli ACL creati in precedenza per le subnet virtuali o connettività del gateway per tenant. 

La tabella seguente include l'ID subnet di esempio e i prefissi per due tenant fittizio. Il tenant Fabrikam ha due subnet virtuali, mentre il tenant Contoso include tre subnet virtuali.  
 
  
Nome del tenant  |ID subnet virtuale  |Virtual Subnet Prefix    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
Lo script di esempio seguente usa i comandi di Windows PowerShell esportati dal **NetworkController** modulo per creare una subnet e rete virtuale di Contoso:   
  
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
È possibile usare Windows PowerShell per aggiornare una rete o subnet virtuale esistente.   
  
Quando si esegue lo script di esempio seguente, le risorse aggiornate vengono semplicemente INSERITE al Controller di rete con lo stesso ID di risorsa. Se il tenant Contoso vuole aggiungere una nuova subnet virtuale (24.30.2.0/24) alla rete virtuale, l'utente o l'amministratore di Contoso può usare lo script seguente.  
  
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
  
Nell'esempio seguente di Windows PowerShell consente di eliminare un rete virtuale del tenant inviando un HTTP delete all'URI dell'ID della risorsa.  

```PowerShell  
Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  
```

