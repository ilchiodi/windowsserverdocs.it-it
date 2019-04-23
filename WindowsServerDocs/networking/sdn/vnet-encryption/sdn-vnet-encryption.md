---
title: Crittografia di rete virtuale
description: Crittografia di rete virtuale consente la crittografia del traffico della rete virtuale tra le macchine virtuali che comunicano tra loro all'interno di subnet contrassegnata come 'Crittografia Enabled'.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: f2f50ae3146854e2ef6081b0c400a474b53dcf66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851082"
---
# <a name="virtual-network-encryption"></a>Crittografia di rete virtuale

>Si applica a: Windows Server

Crittografia di rete virtuale consente la crittografia del traffico della rete virtuale tra le macchine virtuali che comunicano tra loro all'interno di subnet contrassegnata come 'Crittografia Enabled'. Utilizza anche Datagram Transport Layer Security (DTLS) nella subnet virtuale per codificare pacchetti. DTLS impedisce intercettazioni, manomissioni e contraffazioni da parte di chiunque abbia accesso alla rete fisica.

Richiede la crittografia di rete virtuale:
- Certificati di crittografia installati in ogni host Hyper-V SDN abilitata.
- Un oggetto credenziale nel Controller di rete che fa riferimento l'identificazione personale del certificato.
- La configurazione in ogni rete virtuale contiene subnet che richiedono la crittografia.

Dopo aver abilitato la crittografia in una subnet, tutto il traffico di rete in una subnet verrà crittografato automaticamente, oltre a qualsiasi tipo di crittografia a livello di applicazione che può anche avere luogo.  Il traffico che attraversa tra subnet, anche se contrassegnata come crittografata, viene inviato crittografato automaticamente. Tutto il traffico che attraversa il limite di rete virtuale viene anche inviato non crittografato.

>[!TIP]
>Se è necessario limitare alle applicazioni di comunicare solo nella subnet crittografata, è possibile utilizzare elenchi di controllo di accesso (ACL) solo per consentire la comunicazione all'interno della subnet corrente. Per altre informazioni, vedere [elenchi di controllo di accesso utilizzare (ACL) per gestire Data Center rete traffico](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).

### <a name="next-steps"></a>Passaggi successivi

[Configurare la crittografia per una rete virtuale](https://docs.microsoft.com/windows-server/networking/sdn/vnet-encryption/sdn-config-vnet-encryption)

