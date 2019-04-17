---
title: Configurare gli elenchi di controllo di accesso (ACL) Firewall del centro dati
description: Questo argomento fa parte della Guida alla rete definita dal Software su come gestire carichi di lavoro Tenant e reti virtuali in Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: d5f7c4baad24a720e073857cb6c835167e5b419b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>Configurare gli elenchi di controllo di accesso (ACL) Firewall del centro dati

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile applicare gli ACL specifico per le interfacce di rete.  Se gli ACL sono impostati anche per la subnet virtuale a cui è connessa l'interfaccia di rete, sia gli ACL vengono applicati, ma l'interfaccia di rete gli ACL vengono ordinati sopra la subnet virtuale ACL.

In questo argomento contiene le sezioni seguenti.

- [Esempio: Aggiungere un ACL a un'interfaccia di rete](#bkmk_addacl)
- [Esempio: Rimuovere un ACL da un'interfaccia di rete utilizzando Windows Powershell e l'API REST di Controller di rete](#bkmk_removeacl)

##<a name="bkmk_addacl"></a>Esempio: Aggiungere un ACL a un'interfaccia di rete

Nell'argomento [elenchi di controllo di accesso utilizzare (ACL) per gestire Data Center rete traffico](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md) è stato descritto come creare un ACL e assegnarlo a una subnet virtuale.  In alcuni casi, tuttavia, si desidera eseguire l'override di tale ACL predefinito nella subnet virtaul con un ACL specifico per un'interfaccia di rete.  È anche necessario applicare gli ACL direttamente alle interfacce di rete collegate a VLAN invece di reti virtuali.

Questo esempio viene illustrato come aggiungere un ACL a una rete virtuale. 

>[!NOTE]
>È anche possibile aggiungere un ACL allo stesso tempo che crei l'interfaccia di rete.

###<a name="step-1-get-or-create-the-network-interface-to-which-you-will-add-the-acl"></a>Passaggio 1: Ottenere o creare l'interfaccia di rete a cui si desidera aggiungere l'ACL

    $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-2-get-or-create-the-acl-you-will-add-to-the-network-interface"></a>Passaggio 2: Ottenere o creare l'ACL si aggiungerà l'interfaccia di rete
È possibile utilizzare il comando seguente per ottenere o creare l'ACL. 

    $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"

###<a name="step-3-assign-the-acl-to-the-accesscontrollist-property-of-the-network-interface"></a>Passaggio 3: Assegnare l'ACL per la proprietà AccessControlList dell'interfaccia di rete
È possibile utilizzare il comando seguente per assegnare l'ACL per la proprietà AccessControlList.

    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl

###<a name="step-4-add-the-network-interface-in-network-controller"></a>Passaggio 4: Aggiungere l'interfaccia di rete nel Controller di rete
È possibile utilizzare il comando seguente per aggiungere l'interfaccia di rete nel Controller di rete.

    new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid


##<a name="bkmk_removeacl"></a>Esempio: Rimuovere un ACL da un'interfaccia di rete utilizzando Windows Powershell e l'API REST di Controller di rete
È possibile utilizzare questo esempio, se si desidera rimuovere un ACL. Quando si rimuove un ACL, il set predefinito di regole vengono applicate all'interfaccia di rete.

Il set predefinito di regole consente tutto il traffico in uscita, ma blocca tutto il traffico in entrata.

>[!NOTE]
>Se si desidera consentire tutto il traffico in entrata, è necessario seguire l'esempio precedente per aggiungere un ACL che consente di tutto il traffico in entrata e uscito tutti.

###<a name="step-1-get-the-network-interface-from-which-you-will-remove-the-acl"></a>Passaggio 1: Ottenere l'interfaccia di rete da cui rimuovere l'ACL
È possibile utilizzare il comando seguente per recuperare l'interfaccia di rete.

    $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-2-assign-null-to-the-accesscontrollist-property-of-the-ipconfiguration"></a>Passaggio 2: Assegnare $NULL alla proprietà del ipConfiguration AccessControlList
È possibile utilizzare il comando seguente per assegnare la proprietà AccessControlList $NULL.

    $nic.properties.ipconfigurations[0].properties.AccessControlList = $null

###<a name="step-3-add-the-network-interface-object-in-network-controller"></a>Passaggio 3: Aggiungere l'oggetto di interfaccia di rete nel Controller di rete
È possibile utilizzare il comando seguente per aggiungere l'oggetto di interfaccia di rete nel Controller di rete,

    new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid

