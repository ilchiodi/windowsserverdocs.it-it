---
title: Peering di rete virtuale
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: aab4ec7c69ec5b52eae926cd1065d777415b1124
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446215"
---
# <a name="virtual-network-peering"></a>Peering di rete virtuale

>Si applica a: Windows Server

Peering reti virtuali consente di connettere due reti virtuali senza problemi. Una volta eseguito il peering, per motivi di connettività, le reti virtuali vengono visualizzate come una. 

I vantaggi dell'uso di peering di rete virtuale includono:

-   Il traffico tra macchine virtuali nelle reti virtuali con peering viene indirizzato attraverso l'infrastruttura backbone attraverso *privato* solo indirizzi IP. La comunicazione tra le reti virtuali non richiede Internet pubblici o i gateway.

-   Una connessione di bassa latenza e larghezza di banda elevata tra le risorse in reti virtuali diverse.

-   La possibilità per le risorse in una rete virtuale di comunicare con risorse in un'altra rete virtuale.

-   Nessun tempo di inattività per le risorse in qualsiasi rete virtuale quando si crea il peering.

## <a name="requirements-and-constraints"></a>Requisiti e vincoli

Peering di rete virtuale presenta alcuni requisiti e vincoli:

- Reti virtuali con peering devono:

  -   Avere spazi di indirizzi IP non sovrapposte

  -   Essere gestite dallo stesso Controller di rete

- Dopo che è il peering di una rete virtuale con un'altra rete virtuale, è possibile aggiungere o eliminare intervalli di indirizzi nello spazio degli indirizzi.

  >[!TIP]
  >Se è necessario aggiungere gli intervalli di indirizzi:<ol><li>Rimuovere il peering.</li><li>Aggiungere lo spazio degli indirizzi.</li><li>Riaggiungere il peering.</li></ol>

- Poiché il peering di rete virtuale è comprese tra due reti virtuali, non vi è alcuna relazione transitiva derivata nei peering. Ad esempio, se vengono eseguiti con virtualNetworkB e virtualNetworkB con peering è il peering, quindi vengono eseguiti non ottenere il peering con peering.

  [immagine qui]

## <a name="connectivity"></a>Connettività

Dopo che è il peering reti virtuali, le risorse in entrambe le reti virtuali possono connettersi direttamente alle risorse nella rete virtuale con peering.

-   Latenza di rete tra macchine virtuali in reti virtuali con peering è identica a quella all'interno di una singola rete virtuale.

-   Velocità effettiva della rete si basa sulla larghezza di banda consentita per la macchina virtuale. Non è disponibile alcun ulteriore limitazione della larghezza di banda nel peering.

-   Il traffico tra macchine virtuali in reti virtuali con peering viene instradato direttamente tramite l'infrastruttura backbone, non tramite un gateway o sulla rete Internet pubblica.

-   Le macchine virtuali in una rete virtuale può accedere il-bilanciamento del carico interno nella rete virtuale con peering.

È possibile applicare elenchi di controllo di accesso (ACL) in qualsiasi rete virtuale per bloccare l'accesso alle altre reti virtuali o subnet se si desidera. Se si apre la connettività completa tra reti virtuali con peering (che è l'opzione predefinita), è possibile applicare gli ACL per le subnet specifiche o le macchine virtuali per bloccare o negare un accesso specifico. Per altre informazioni sugli ACL, vedere [elenchi di controllo di accesso utilizzare (ACL) per gestire Data Center rete traffico](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).

## <a name="service-chaining"></a>Concatenamento dei servizi

È possibile configurare route definite dall'utente che puntano a macchine virtuali in reti virtuali con peering come l'indirizzo IP hop successivo, per abilitare il concatenamento di servizio. Concatenamento dei servizi consente di indirizzare il traffico da una rete virtuale a un'appliance virtuale, in una rete virtuale con peering, tramite route definite dall'utente.

È possibile distribuire reti hub e spoke, in cui la rete virtuale hub può ospitare componenti dell'infrastruttura, ad esempio un'appliance virtuale di rete. Tutte le reti virtuali spoke il peering con la rete virtuale dell'hub. Il traffico può fluire attraverso le Appliance virtuali di rete nella rete virtuale dell'hub.

Peering reti virtuali consente l'hop successivo in una route definita dall'utente come indirizzo IP di una macchina virtuale nella rete virtuale con peering. Per altre informazioni sulle route definite dall'utente, vedere [usare Appliance virtuali di rete in una rete virtuale](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn).

## <a name="gateways-and-on-premises-connectivity"></a>Connettività da sito locale e i gateway

Ogni rete virtuale, di indipendentemente dal fatto che il peering con un'altra rete virtuale, può comunque avere un proprio gateway per connettersi a una rete locale. Quando è il peering reti virtuali, è anche possibile configurare il gateway nella rete virtuale con peering come punto di transito a una rete locale. In questo caso, la rete virtuale che usa un gateway remoto non può avere un proprio gateway. Una rete virtuale può avere un solo gateway che può essere locale o remoto (nella rete virtuale con peering).

## <a name="monitor"></a>Monitoraggio

Quando si eseguire il peering di due reti virtuali, è necessario configurare un peering per ogni rete virtuale nel peering.

È possibile monitorare lo stato della connessione peering, che può essere in uno dei seguenti stati:

-   **Avviata da:** Visualizzato quando si crea il peering dalla prima rete virtuale alla seconda rete virtuale.

-   **Connessione:** Visualizzato dopo aver creato il peering dalla seconda rete virtuale alla prima rete virtuale. Lo stato del peering per la prima rete virtuale cambia da avviato a connesso. Entrambi i peer di rete virtuale devono avere lo stato della connessione prima di stabilire una peering correttamente di rete virtuale.

-   **Disconnesso:** Visualizzato se una rete virtuale disconnette da un'altra rete virtuale.

[infografica degli stati]

## <a name="next-steps"></a>Passaggi successivi
[Configurare il peering di rete virtuale](sdn-configure-vnet-peering.md): In questa procedura, si usa Windows PowerShell per trovare la virtualizzazione di rete rete logica del provider per creare due reti virtuali, ognuna con una subnet. È anche possibile configurare il peering tra le due reti virtuali.

