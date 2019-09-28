---
title: Configurare gli elenchi di controllo di accesso (ACL) del firewall del centro dati
description: È possibile applicare ACL specifici alle interfacce di rete.  Se anche gli ACL sono impostati sulla subnet virtuale a cui è connessa l'interfaccia di rete, vengono applicati entrambi gli ACL, ma gli ACL dell'interfaccia di rete vengono classificati in ordine di priorità sopra gli ACL della subnet virtuale.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 25f18927-a63e-44f3-b02a-81ed51933187
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 2e3f365a820de67dec87ea209cbfeb22d9091616
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406109"
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>Configurare gli elenchi di controllo di accesso (ACL) del firewall del Data Center

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Dopo aver creato un ACL e averlo assegnato a una subnet virtuale, potrebbe essere necessario eseguire l'override dell'ACL predefinito nella subnet virtuale con un ACL specifico per una singola interfaccia di rete.  In questo caso, si applicano ACL specifici direttamente alle interfacce di rete collegate alle VLAN, anziché alla rete virtuale. Se sono stati impostati ACL sulla subnet virtuale connessa all'interfaccia di rete, vengono applicati entrambi gli ACL e viene assegnata la priorità agli ACL dell'interfaccia di rete sopra gli ACL della subnet virtuale.

>[!IMPORTANT]
>Se non è stato creato un ACL e lo si è assegnato a una rete virtuale, vedere [usare gli elenchi di controllo di accesso (ACL) per gestire il flusso del traffico di rete del Data Center](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md) per creare un ACL e assegnarlo a una subnet virtuale.  

In questo argomento viene illustrato come aggiungere un ACL a un'interfaccia di rete. Viene inoltre illustrato come rimuovere un ACL da un'interfaccia di rete tramite Windows PowerShell e l'API REST del controller di rete.

- [Esempio: Aggiungere un ACL a un'interfaccia di rete @ no__t-0
- [Esempio: Rimuovere un ACL da un'interfaccia di rete usando Windows PowerShell e l'API REST del controller di rete @ no__t-0


## <a name="example-add-an-acl-to-a-network-interface"></a>Esempio: Aggiungere un ACL a un'interfaccia di rete
In questo esempio viene illustrato come aggiungere un ACL a una rete virtuale. 

>[!TIP]
>È anche possibile aggiungere un ACL nello stesso momento in cui si crea l'interfaccia di rete.

1. Ottenere o creare l'interfaccia di rete a cui si aggiungerà l'ACL.
 
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. Ottenere o creare l'ACL che si aggiungerà all'interfaccia di rete.
 
   ```PowerShell
   $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"
   ```
 
3. Assegnare l'ACL alla Proprietà AccessControlList dell'interfaccia di rete
 
   ```PowerShell
    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl
   ```
 
4. Aggiungere l'interfaccia di rete nel controller di rete
 
   ```
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
 
## <a name="example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api"></a>Esempio: Rimuovere un ACL da un'interfaccia di rete usando Windows PowerShell e l'API REST del controller di rete
In questo esempio viene illustrato come rimuovere un ACL. La rimozione di un ACL applica il set predefinito di regole all'interfaccia di rete. Il set predefinito di regole consente tutto il traffico in uscita, ma blocca tutto il traffico in ingresso.

>[!NOTE]
>Per consentire tutto il traffico in ingresso, è necessario seguire l' [esempio](#example-add-an-acl-to-a-network-interface) precedente per aggiungere un ACL che consente tutto il traffico in ingresso e tutto il traffico in uscita.


1. Ottenere l'interfaccia di rete da cui si rimuove l'ACL.<br>
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. Assegnare $NULL alla Proprietà AccessControlList di configurazione IP.<br>
   ```PowerShell
   $nic.properties.ipconfigurations[0].properties.AccessControlList = $null
   ```
 
3. Aggiungere l'oggetto interfaccia di rete nel controller di rete.<br>
   ```PowerShell
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
