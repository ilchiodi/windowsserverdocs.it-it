---
title: Creare, Delete o Update reti virtuali Tenant
description: Questo argomento fa parte della Guida alla rete definita dal Software su come gestire carichi di lavoro Tenant e reti virtuali in Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: 6ef30dcc31593e15c36f846cf6d64afcd4b85f19
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>Creare, Delete o Update reti virtuali Tenant

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come creare, eliminare e aggiornare le reti virtuali di Hyper-V rete virtualizzazione dopo la distribuzione di rete SDN (Software Defined).  
  
Con virtualizzazione rete Hyper-V, è possibile isolare le reti tenant in modo che ogni rete tenant è un'entità completamente separata con alcuna possibilità di cross-connessione a meno che non si configura accesso pubblico i carichi di lavoro.  
  
È possibile creare nuove reti virtuali per tenant, è possibile modificare le reti virtuali esistenti e se un tenant non è più necessario alcune risorse, o se il tenant non è più del cliente, è possibile eliminare le reti virtuali tenant.  
  
## <a name="create-a-new-virtual-network"></a>Creare una nuova rete virtuale  
  
Quando si crea una rete virtuale per un tenant, si trova all'interno di un dominio di routing univoco nell'host Hyper-V.  
  
Di seguito sono i passaggi per creare una nuova rete virtuale.  
  
1. Identificare i prefissi di indirizzo IP da cui si desidera creare le subnet virtuali.   
2. Identificare la rete logica provider in cui viene effettuato il tunneling il traffico tenant.   
3. Creare almeno una subnet virtuale per ogni prefisso IP definito nel passaggio 1.   
  
>[!NOTE]  
>Di sotto di ogni rete virtuale esiste almeno una subnet virtuale. Subnet virtuali sono definite da un prefisso IP e fare riferimento a un elenco di controllo di accesso definito in precedenza.  
  
Facoltativamente, dopo aver completato questi passaggi, puoi anche aggiungere gli elenchi di controllo di accesso creato in precedenza per le subnet virtuali, o aggiungere connettività gateway per tenant.    
  
La tabella seguente include l'ID di subnet di esempio e prefissi per due tenant fittizio. Tenant Fabrikam ha due subnet virtuali, mentre il tenant di Contoso ha tre subnet virtuali.  
  
  
  
Nome del tenant  |ID Subnet virtuale  |Prefisso di Subnet virtuale    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
Lo script di esempio seguente usa i comandi di Windows PowerShell esportati dal **NetworkController** modulo per creare una rete virtuale e una subnet di Contoso:   
  
```  
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
È possibile utilizzare Windows PowerShell per aggiornare una rete o subnet virtuale esistente.   
  
Quando si esegue lo script di esempio seguente, le risorse aggiornate sono essenzialmente al Controller di rete con lo stesso ID di risorsa. Se il tenant Contoso desidera aggiungere una nuova subnet virtuale (24.30.2.0/24) alla rete virtuale, l'utente o l'amministratore di Contoso può usare lo script seguente.  
  
```  
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
  
L'esempio seguente di Windows PowerShell Elimina un rete virtuale del tenant eseguendo un'eliminazione HTTP all'URI dell'ID di risorsa.  
  
    Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  


