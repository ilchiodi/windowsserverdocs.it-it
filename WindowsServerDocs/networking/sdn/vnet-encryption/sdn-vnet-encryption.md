---
title: Crittografia di rete virtuale
description: Questo argomento fornisce informazioni sulla crittografia di rete virtuale per la rete definita dal Software in Windows Server
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: pashort
author: grcusanz
ms.openlocfilehash: 425cc1cff8c7241aeec7764b60c89b4586fd323b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="virtual-network-encryption"></a>Crittografia di rete virtuale

>Si applica a: Windows Server

Crittografia di rete virtuale offre la possibilità per il traffico di rete virtuale essere crittografati tra le macchine virtuali in grado di comunicare tra loro all'interno di subnet che sono contrassegnate come "La crittografia abilitata".

Questa funzionalità Usa Datagram Transport Layer Security (DTLS) nella subnet virtuale per crittografare i pacchetti.  DTLS fornisce protezione contro l'intercettazione, la manomissione e i falsi da chiunque abbia accesso alla rete fisica.

Requries crittografia di rete virtuale un certificato di crittografia per essere installati in tutti i SDN abilitati gli host Hyper-V, un oggetto credenziali nel Controller di rete che fa riferimento l'identificazione personale del certificato e tale configurazione in ognuna delle reti virtuali che contengono subnet obbligo di crittografia.

Dopo aver abilitata la crittografia in una subnet, tutto il traffico di rete all'interno di tale subnet verrà automaticamente crittografato.  Questa sarà oltre a qualsiasi crittografia a livello di applicazione che può anche avere luogo.  Il traffico che attraversa tra le subnet, anche se entrambe le subnet sono contrassegnati come crittografata viene inviato automaticamente crittografato.  Tutto il traffico che attraversa il limite di rete virtuale viene inviato anche non crittografato.

Per informazioni sulla configurazione di crittografia di rete virtuale, vedere [configurare la crittografia per una rete virtuale](sdn-config-vnet-encryption.md).

Se è necessario limitare le applicazioni di comunicare solo nella subnet crittografata.  È possibile utilizzare elenchi di controllo di accesso (ACL) per consentire solo la comunicazione all'interno della subnet corrente.  

Per informazioni sulla configurazione di elenchi di controllo di accesso, vedere [elenchi di controllo di accesso utilizzare (ACL) per gestire Data Center rete traffico](../manage/use-acls-for-traffic-flow.md).