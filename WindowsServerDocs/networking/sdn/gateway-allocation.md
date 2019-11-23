---
title: Allocazione della larghezza di banda del gateway
description: ''
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: fc59fc9d7dc22b9c5567bae314b4312d76fcff19
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406125"
---
# <a name="gateway-bandwidth-allocation"></a>Allocazione della larghezza di banda del gateway

>Si applica a: Windows Server

In Windows Server 2016, la larghezza di banda dei singoli tunnel per IPsec, GRE e L3 era un rapporto tra la capacità totale del gateway. I clienti forniranno quindi la capacità del gateway in base alla larghezza di banda TCP standard prevista dalla VM del gateway.

Inoltre, la larghezza di banda massima del tunnel IPsec sul gateway è stata limitata a (3/20)\*capacità del gateway fornita dal cliente. Se, ad esempio, si imposta la capacità del gateway su 100 Mbps, la capacità del tunnel IPsec sarà 150 Mbps. I rapporti equivalenti per i tunnel GRE e L3 sono rispettivamente 1/5 e 1/2.

Sebbene questo funzionava per la maggior parte delle distribuzioni, il modello a rapporto fisso non era appropriato per ambienti con velocità effettiva elevata. Anche quando le velocità di trasferimento dei dati sono elevate (ad indicare più di 40 Gbps), la velocità effettiva massima dei tunnel del gateway SDN è limitata a causa di fattori interni.

In Windows Server 2019, per un tipo di tunnel, viene fissata la velocità effettiva massima:

-   IPsec = 5 Gbps

-   GRE = 15 Gbps

-   L3 = 5 Gbps

Quindi, anche se l'host/VM del gateway supporta le schede di rete con una velocità effettiva molto più elevata, viene fissata la velocità effettiva massima disponibile per il tunnel. Un altro problema che si verifica è che il provisioning dei gateway sia arbitrario, che si verifica quando si fornisce un numero molto elevato per la capacità del gateway.

## <a name="gateway-capacity-calculation"></a>Calcolo della capacità del gateway

Idealmente, è possibile impostare la capacità della velocità effettiva del gateway sulla velocità effettiva disponibile per la macchina virtuale del gateway. Quindi, ad esempio, se si dispone di una singola macchina virtuale del gateway e la velocità effettiva della scheda di interfaccia di rete dell'host sottostante è 25 Gbps, la velocità effettiva del gateway può essere impostata anche su 25 Gbps.

Se si usa un gateway solo per le connessioni IPsec, la capacità fissa massima disponibile è di 5 Gbps. Se, ad esempio, si esegue il provisioning di connessioni IPsec sul gateway, è possibile effettuare il provisioning solo di una larghezza di banda di aggregazione (in ingresso + in uscita) a 5 Gbps.

Se si usa il gateway per la connettività IPsec e GRE, è possibile effettuare il provisioning di un massimo di 5 Gbps di velocità effettiva IPsec o massimo 15 Gbps di velocità effettiva GRE. Se, ad esempio, si esegue il provisioning di 2 Gbps della velocità effettiva IPsec, si avranno a disposizione 3 Gbps di velocità effettiva IPsec fino a quando non si esegue il provisioning sul gateway o a 9 Gbps.

Per inserire questo in termini più matematici:

- Capacità totale del gateway = 25 Gbps

- Capacità IPsec totale disponibile = 5 Gbps (fisso)

- Capacità totale di GRE disponibile = 15 Gbps (fisso)

- Rapporto velocità effettiva IPsec per questo gateway = 25/5 = 5 Gbps

- Rapporto velocità effettiva GRE per questo gateway = 25/15 = 5/3 Gbps

Ad esempio, se si allocano 2 Gbps di velocità effettiva IPsec a un cliente:

Capacità rimanente disponibile sul gateway = capacità totale del gateway-rapporto velocità effettiva IPsec * velocità effettiva IPsec allocata (capacità utilizzata)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;25-5 * 2 = 15 Gbps

Velocità effettiva IPsec rimanente che è possibile allocare sul gateway 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5-2 = 3 Gbps

Velocità effettiva rimanente del GRE che è possibile allocare sul gateway = capacità rimanente del rapporto velocità effettiva gateway/GRE 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15 * 3/5 = 9 Gbps

Il rapporto di velocità effettiva varia a seconda della capacità totale del gateway. Una cosa da notare è che è necessario impostare la capacità totale per la larghezza di banda TCP disponibile per la macchina virtuale del gateway. Se nel gateway sono ospitate più macchine virtuali, è necessario modificare di conseguenza la capacità totale del gateway.

Inoltre, se la capacità del gateway è inferiore alla capacità totale del tunnel disponibile, la capacità del tunnel totale disponibile viene impostata sulla capacità del gateway. Se ad esempio si imposta la capacità del gateway su 4 Gbps, la capacità totale disponibile per IPsec, L3 e GRE è impostata su 4 Gbps, lasciando il rapporto di velocità effettiva per ogni tunnel a 1 Gbps.

## <a name="windows-server-2016-behavior"></a>Comportamento di Windows Server 2016

L'algoritmo di calcolo della capacità del gateway per Windows Server 2016 rimane invariato. In Windows Server 2016, la larghezza di banda massima del tunnel IPsec è stata limitata a (3/20)\*capacità del gateway in un gateway. I rapporti equivalenti per i tunnel GRE e L3 sono rispettivamente 1/5 e 1/2.

Se si esegue l'aggiornamento da Windows Server 2016 a Windows Server 2019:

1.  **Tunnel GRE e L3:** La logica di allocazione di Windows Server 2019 viene applicata quando i nodi del controller di rete vengono aggiornati a Windows Server 2019

2.  **Tunnel IPSec:** La logica di allocazione del gateway Windows Server 2016 continua a funzionare fino a quando tutti i gateway nel pool di gateway non vengono aggiornati a Windows Server 2019. Per tutti i gateway nel pool di gateway, è necessario impostare il servizio gateway di Azure su **automatico**.

>[!NOTE]
>Dopo l'aggiornamento a Windows Server 2019, è possibile che un gateway venga sottoposta a provisioning (in quanto la logica di allocazione passa da Windows Server 2016 a Windows Server 2019). In questo caso, le connessioni esistenti sul gateway continuano a esistere. La risorsa REST per il gateway genera un avviso che indica che è stato effettuato il provisioning eccessivo del gateway. In questo caso, è necessario spostare alcune connessioni a un altro gateway.

---