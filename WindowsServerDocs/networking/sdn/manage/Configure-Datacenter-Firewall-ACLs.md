---
title: Configurare gli elenchi di controllo di accesso (ACL) del firewall del centro dati
description: È possibile applicare un ACL specifico alle interfacce di rete.  Se gli ACL vengono inoltre impostati nella subnet virtuale a cui è connessa l'interfaccia di rete, sia gli ACL vengono applicati, ma l'interfaccia di rete ACL hanno priorità maggiore sopra la subnet virtuale ACL.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 25f18927-a63e-44f3-b02a-81ed51933187
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 77a7706e39da265eedd65342a0ccf2174ab050ea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853402"
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>Configurare firewall del centro dati elenchi di controllo di accesso (ACL)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Dopo aver creato un ACL e assegnarlo a una subnet virtuale, è possibile eseguire l'override di tale ACL predefinito della subnet virtuale con un ACL specifico per un'interfaccia di rete individuali.  In questo caso, applicare gli ACL specifici direttamente a interfacce di rete collegate a reti VLAN, anziché la rete virtuale. Se si dispone di ACL impostato nella subnet virtuale connessa all'interfaccia di rete, entrambi gli ACL vengono applicati e assegna la priorità l'interfaccia di rete ACL sopra gli ACL di subnet virtuale.

>[!IMPORTANT]
>Se non si hanno creato un ACL e averlo assegnato a una rete virtuale, vedere [elenchi di controllo di accesso utilizzare (ACL) per gestire Data Center rete traffico](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md) per creare un ACL e assegnarla a una subnet virtuale.  

In questo argomento, mostreremo come aggiungere un ACL a un'interfaccia di rete. Anche mostreremo come rimuovere un ACL da un'interfaccia di rete tramite Windows PowerShell e l'API REST del Controller di rete.

- [Esempio: Aggiungere un ACL a un'interfaccia di rete](#example-add-an-acl-to-a-network-interface)
- [Esempio: Rimuovere un ACL da un'interfaccia di rete utilizzando Windows Powershell e l'API REST del Controller di rete](#example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api)


## <a name="example-add-an-acl-to-a-network-interface"></a>Esempio: Aggiungere un ACL a un'interfaccia di rete
In questo esempio si illustra come aggiungere un ACL a una rete virtuale. 

>[!TIP]
>È anche possibile aggiungere un ACL allo stesso tempo che si crea l'interfaccia di rete.

1. Ottiene o creare l'interfaccia di rete a cui si aggiungerà l'ACL.
 
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. Ottiene o creare l'ACL si aggiungeranno all'interfaccia di rete.
 
   ```PowerShell
   $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"
   ```
 
3. Assegnare l'ACL per la proprietà oggetto AccessControlList dell'interfaccia di rete
 
   ```PowerShell
    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl
   ```
 
4. Aggiungere l'interfaccia di rete nel Controller di rete
 
   ```
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
 
## <a name="example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api"></a>Esempio: Rimuovere un ACL da un'interfaccia di rete utilizzando Windows Powershell e l'API REST del Controller di rete
In questo esempio viene illustrato come rimuovere un ACL. Rimozione di un ACL si applica il set predefinito di regole per l'interfaccia di rete. Il set predefinito di regole consente tutto il traffico in uscita ma blocca tutto il traffico in ingresso.

>[!NOTE]
>Se si desidera consentire tutto il traffico in ingresso, è necessario seguire il precedente [esempio](#example-add-an-acl-to-a-network-interface) per aggiungere un ACL che consenta il traffico in ingresso tutti e tutte in uscita.


1. Ottenere l'interfaccia di rete da cui verrà rimosso l'ACL.<br>
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. Assegnare $NULL alla proprietà oggetto AccessControlList della configurazione di IP.<br>
   ```PowerShell
   $nic.properties.ipconfigurations[0].properties.AccessControlList = $null
   ```
 
3. Aggiungere l'oggetto di interfaccia di rete nel Controller di rete.<br>
   ```PowerShell
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
