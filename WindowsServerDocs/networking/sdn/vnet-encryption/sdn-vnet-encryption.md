---
title: Crittografia di rete virtuale
description: La crittografia della rete virtuale consente la crittografia del traffico di rete virtuale tra macchine virtuali che comunicano tra loro all'interno delle subnet contrassegnate come "crittografia abilitata".
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/08/2018
ms.openlocfilehash: 63daea02ec00593504383ce071d3f9454a37956b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853574"
---
# <a name="virtual-network-encryption"></a>Crittografia di rete virtuale

>Si applica a: Windows Server

La crittografia della rete virtuale consente la crittografia del traffico di rete virtuale tra macchine virtuali che comunicano tra loro all'interno delle subnet contrassegnate come "crittografia abilitata". Viene inoltre utilizzato Datagram Transport Layer Security (DTLS) nella subnet virtuale per crittografare i pacchetti. DTLS impedisce intercettazioni, manomissioni e contraffazioni da parte di chiunque abbia accesso alla rete fisica.

La crittografia della rete virtuale richiede:
- Certificati di crittografia installati in ognuno degli host Hyper-V abilitati per SDN.
- Un oggetto credenziale nel controller di rete che fa riferimento all'identificazione personale del certificato.
- La configurazione in ogni rete virtuale contiene subnet che richiedono la crittografia.

Una volta abilitata la crittografia in una subnet, tutto il traffico di rete all'interno di tale subnet viene crittografato automaticamente, oltre a qualsiasi crittografia a livello di applicazione che potrebbe essere eseguita.  Il traffico che si interseca tra le subnet, anche se contrassegnato come crittografato, viene inviato automaticamente non crittografato. Anche il traffico che supera il limite della rete virtuale viene inviato non crittografato.

>[!TIP]
>Se è necessario limitare la comunicazione delle applicazioni solo sulla subnet crittografata, è possibile usare gli elenchi di controllo di accesso (ACL) solo per consentire la comunicazione all'interno della subnet corrente. Per altre informazioni, vedere [usare elenchi di controllo di accesso (ACL) per gestire il flusso del traffico di rete del Data Center](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).

### <a name="next-steps"></a>Passaggi successivi

[Configurare la crittografia per una rete virtuale](https://docs.microsoft.com/windows-server/networking/sdn/vnet-encryption/sdn-config-vnet-encryption)

