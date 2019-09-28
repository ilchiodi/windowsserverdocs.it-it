---
title: Peering di rete virtuale
description: ''
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: ccdcbb953939345ef5e9a45dff87fc7af62eb7bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355491"
---
# <a name="virtual-network-peering"></a>Peering di rete virtuale

>Si applica a: Windows Server

Il peering di rete virtuale consente di connettere facilmente due reti virtuali. Una volta eseguito il peering, per finalità di connettività, le reti virtuali vengono visualizzate come una sola. 

I vantaggi dell'utilizzo del peering di rete virtuale includono:

-   Il traffico tra le macchine virtuali nelle reti virtuali con peering viene instradato tramite l'infrastruttura backbone solo tramite indirizzi IP *privati* . Per la comunicazione tra le reti virtuali non sono necessari Internet pubblico o gateway.

-   Connessione a bassa latenza e larghezza di banda elevata tra le risorse in reti virtuali diverse.

-   Possibilità per le risorse in una rete virtuale di comunicare con le risorse in una rete virtuale diversa.

-   Nessun tempo di inattività per le risorse in una rete virtuale durante la creazione del peering.

## <a name="requirements-and-constraints"></a>Requisiti e vincoli

Il peering di rete virtuale presenta alcuni requisiti e vincoli:

- Le reti virtuali con peering devono:

  -   Avere spazi di indirizzi IP non sovrapposti

  -   Essere gestito dallo stesso controller di rete

- Quando si esegue il peering di una rete virtuale con un'altra rete virtuale, non è possibile aggiungere o eliminare intervalli di indirizzi nello spazio degli indirizzi.

  >[!TIP]
  >Se è necessario aggiungere gli intervalli di indirizzi:<ol><li>Rimuovere il peering.</li><li>Aggiungere lo spazio di indirizzi.</li><li>Aggiungere di nuovo il peering.</li></ol>

- Poiché il peering di rete virtuale è tra due reti virtuali, non esiste alcuna relazione transitiva derivata tra i peering. Se, ad esempio, si esegue il peering di vengono eseguiti con il peering e il peering con virtualNetworkC, vengono eseguiti non viene sottomesso a peering con virtualNetworkC.

  [immagine qui]

## <a name="connectivity"></a>Connettività

Dopo aver eseguito il peering delle reti virtuali, le risorse in una delle due reti virtuali possono connettersi direttamente alle risorse nella rete virtuale con peering.

-   La latenza di rete tra le macchine virtuali nelle reti virtuali con peering è identica alla latenza all'interno di una singola rete virtuale.

-   La velocità effettiva della rete è basata sulla larghezza di banda consentita per la macchina virtuale. Non esiste alcuna restrizione aggiuntiva per la larghezza di banda all'interno del peering.

-   Il traffico tra le macchine virtuali nelle reti virtuali con peering viene instradato direttamente tramite l'infrastruttura backbone, non tramite un gateway o attraverso la rete Internet pubblica.

-   Le macchine virtuali in una rete virtuale possono accedere al servizio di bilanciamento del carico interno nella rete virtuale con peering.

È possibile applicare elenchi di controllo di accesso (ACL) in una delle reti virtuali per bloccare l'accesso ad altre reti virtuali o subnet, se lo si desidera. Se si apre la connettività completa tra reti virtuali con peering (opzione predefinita), è possibile applicare ACL a subnet o macchine virtuali specifiche per bloccare o negare un accesso specifico. Per altre informazioni sugli ACL, vedere [usare elenchi di controllo di accesso (ACL) per gestire il flusso del traffico di rete del Data Center](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).

## <a name="service-chaining"></a>Concatenamento di servizi

È possibile configurare route definite dall'utente che puntano a macchine virtuali in reti virtuali con peering come indirizzo IP hop successivo, per abilitare il concatenamento dei servizi. Il concatenamento dei servizi consente di indirizzare il traffico da una rete virtuale a un'appliance virtuale, in una rete virtuale con peering, tramite route definite dall'utente.

È possibile distribuire reti hub e spoke, in cui la rete virtuale dell'hub può ospitare componenti dell'infrastruttura, ad esempio un'appliance virtuale di rete. Tutte le reti virtuali spoke sono peer con la rete virtuale dell'hub. Il traffico può fluire attraverso le appliance virtuali di rete nella rete virtuale dell'hub.

Il peering di rete virtuale consente all'hop successivo in una route definita dall'utente di essere l'indirizzo IP di una macchina virtuale nella rete virtuale con peering. Per altre informazioni sulle route definite dall'utente, vedere [usare le appliance virtuali di rete in una rete virtuale](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn).

## <a name="gateways-and-on-premises-connectivity"></a>Gateway e connettività locale

Ogni rete virtuale, indipendentemente dal fatto che il peering con un'altra rete virtuale, può comunque avere un proprio gateway per connettersi a una rete locale. Quando si esegue il peering di reti virtuali, è anche possibile configurare il gateway nella rete virtuale con peering come punto di transito a una rete locale. In questo caso, la rete virtuale che usa un gateway remoto non può avere un proprio gateway. Una rete virtuale può avere un solo gateway che può essere un gateway locale o remoto (nella rete virtuale con peering).

## <a name="monitor"></a>Monitoraggio

Quando si esegue il peering di due reti virtuali, è necessario configurare un peering per ogni rete virtuale nel peering.

È possibile monitorare lo stato della connessione di peering, che può trovarsi in uno degli Stati seguenti:

-   **Avviato** Visualizzato quando si crea il peering dalla prima rete virtuale alla seconda rete virtuale.

-   **Connesso** Visualizzato dopo aver creato il peering dalla seconda rete virtuale alla prima rete virtuale. Lo stato del peering per la prima rete virtuale cambia da avviato a connesso. Entrambi i peer della rete virtuale devono avere lo stato connesso prima di stabilire correttamente un peering di rete virtuale.

-   **Disconnesso** Visualizzato se una rete virtuale si disconnette da un'altra rete virtuale.

[infografica degli Stati]

## <a name="next-steps"></a>Passaggi successivi
[Configurare il peering di rete virtuale](sdn-configure-vnet-peering.md): In questa procedura viene usato Windows PowerShell per trovare la rete logica del provider HNV per creare due reti virtuali, ognuna con una subnet. Il peering viene inoltre configurato tra le due reti virtuali.

