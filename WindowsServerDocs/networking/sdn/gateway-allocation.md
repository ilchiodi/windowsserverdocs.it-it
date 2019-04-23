---
title: Allocazione della larghezza di banda del gateway
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: 3c259b96e1a8ee27888a5cccc50b285a2f7cb8c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850432"
---
# <a name="gateway-bandwidth-allocation"></a>Allocazione della larghezza di banda del gateway

>Si applica a: Windows Server

In Windows Server 2016, la larghezza di banda del tunnel singolo per IPsec, GRE e L3 era un rapporto tra la capacità totale gateway. Di conseguenza, i clienti fornisce la capacità del gateway basata sulla larghezza di banda TCP standard si aspetta la macchina virtuale gateway.

Inoltre, era limitata a (3/20) IPsec del tunnel larghezza di banda massima nel gateway\*capacità del Gateway fornito dal cliente. Quindi, ad esempio, se si imposta la capacità del gateway a 100 Mbps, la capacità del tunnel IPsec sarà 150 Mbps. I rapporti equivalenti per i tunnel GRE e L3 sono 1, 5 e 1 o 2, rispettivamente.

Anche se era consentito per la maggior parte delle distribuzioni, il modello di rapporto fisso non è appropriato per gli ambienti ad alta velocità effettiva. Anche quando le velocità di trasferimento dei dati sono state elevate (ad esempio, maggiore di 40 Gbps), la velocità effettiva massima di tunnel gateway SDN limitata a causa di fattori interni.

In Windows Server 2019, per un tipo di tunnel, è fissa la velocità effettiva massima:

-   IPsec = 5 Gbps

-   GRE = 15 Gbps

-   L3 = 5 Gbps

Pertanto, anche se l'host o macchina virtuale gateway supporta interfacce di rete con velocità effettiva notevolmente superiore, la velocità effettiva massima tunnel disponibile è fisso. Questo si occupa di un altro problema è in modo arbitrario un provisioning eccessivo gateway, che si verifica quando si fornisce un numero molto elevato per la capacità del gateway.

## <a name="gateway-capacity-calculation"></a>Calcolo della capacità di gateway

In teoria, è impostare la capacità di velocità effettiva del gateway per la velocità effettiva disponibile per la macchina virtuale gateway. Quindi, ad esempio, se si dispone di un singolo gateway della macchina virtuale e la velocità effettiva sottostante interfaccia di rete host è di 25 Gbps, la velocità effettiva gateway impostabili a 25 Gbps anche.

Se si usa un gateway solo per le connessioni IPsec, la capacità massima disponibile predefinita è 5 Gbps. Pertanto, ad esempio, se si esegue il provisioning di connessioni IPsec nel gateway, può effettuare il provisioning solo a una larghezza di banda aggregata (in ingresso e in uscita) come 5 Gbps.

Se si usa il gateway per la connettività sia IPsec e GRE, è possibile fornire la massima velocità effettiva 5 Gbps di IPsec o la velocità massima effettiva Gbps di GRE 15. Ad esempio, se si esegue il provisioning della velocità effettiva di 2 Gbps di IPsec, è necessario 3 Gbps di IPsec velocità effettiva da sinistra a effettuare il provisioning del gateway o velocità effettiva di 9 GB/s di GRE a sinistra.

Per inserire il codice seguente in termini più matematiche:

- Totale capacità del gateway = 25 Gbps

- Totale capacità IPsec disponibile = 5 Gbps (fisso)

- Totale capacità GRE disponibile = 15 Gbps (fisso)

- Rapporto tra velocità effettiva di IPsec per il gateway di = 5/25 = 5 Gbps

- Rapporto tra velocità effettiva GRE per questo gateway = 15/25 = 5/3 GB/s

Ad esempio, se si alloca la velocità effettiva di 2 Gbps di IPsec a un cliente:

Capacità disponibile nel gateway rimanente = capacità totale del gateway: rapporto tra velocità effettiva di IPsec * IPsec della velocità effettiva allocata (capacità utilizzata)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;25–5*2 = 15 Gbps

Velocità effettiva di IPsec rimanente che è possibile allocare nel gateway 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5-2 = 3 Gbps

Velocità effettiva GRE rimanente che è possibile allocare nel gateway = capacità rimanente di rapporto di velocità effettiva/GRE del gateway 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15*3/5 = 9 Gbps

Il rapporto tra velocità effettiva varia a seconda della capacità totale del gateway. Un aspetto da sottolineare è che è necessario impostare la capacità totale su larghezza di banda TCP disponibile per la macchina virtuale gateway. Se si dispone di più macchine virtuali ospitate nel gateway, è necessario regolare la capacità totale del gateway.

Inoltre, se la capacità del gateway è inferiore alla capacità totale disponibile tunnel, la capacità totale disponibile tunnel è impostata per la capacità del gateway. Ad esempio, se si imposta la capacità del gateway a 4 GB/s, la capacità totale disponibile per IPsec e L3 GRE è impostata a 4 GB/s, lasciando il rapporto tra velocità effettiva per ogni tunnel a 1 Gbps.

## <a name="windows-server-2016-behavior"></a>Comportamento di Windows Server 2016

L'algoritmo di calcolo della capacità di gateway per Windows Server 2016 rimane invariato. In Windows Server 2016, era limitato a (3/20) della larghezza di banda tunnel IPsec massimo\*capacità del gateway in un gateway. I rapporti equivalenti per i tunnel GRE e L3 sono state 1, 5 e 1 o 2, rispettivamente.

Se esegue l'aggiornamento da Windows Server 2016 per Windows Server 2019:

1.  **Tunnel GRE e L3:** La logica di allocazione di Windows Server 2019 diventa effettiva dopo aver aggiornati i nodi di Controller di rete per Windows Server 2019

2.  **Tunnel IPSec:** La logica di allocazione gateway Windows Server 2016 continua a funzionare fino a quando tutti i gateway nel pool di gateway è stati aggiornati a Windows Server 2019. Per tutti i gateway nel pool di gateway, è necessario impostare il servizio gateway di Azure **automatica**.

>[!NOTE]
>È possibile che, dopo l'aggiornamento a Windows Server 2019, un gateway diventa con provisioning eccessivo (se la logica di allocazione viene modificato da Windows Server 2016 per Windows Server 2019). In questo caso, le connessioni esistenti nel gateway comunque disponibili. La risorsa REST per il Gateway genera un avviso che il gateway sia con provisioning eccessivo. In questo caso, è necessario spostare alcune connessioni a un altro gateway.

---