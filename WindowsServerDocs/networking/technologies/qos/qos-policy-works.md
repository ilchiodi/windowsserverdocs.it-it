---
title: Funzionamento dei criteri QoS
description: In questo argomento viene fornita una panoramica dei criteri di qualità del servizio (QoS), che consentono di usare criteri di gruppo per definire le priorità della larghezza di banda di rete del traffico di specifiche applicazioni e servizi in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 272272c833bb38924f1daa5561037901f6ff4e25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864282"
---
# <a name="how-qos-policy-works"></a>Funzionamento dei criteri QoS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Quando avvio ottenuto utente aggiornate o le impostazioni di criteri di gruppo Configurazione computer per QoS o, si verifica quanto segue.

1. Il motore di criteri di gruppo consente di recuperare le impostazioni di criteri di gruppo Configurazione computer o utente da Active Directory.

2. Il motore di criteri di gruppo indica l'estensione lato Client QoS che esistevano modifiche nei criteri di QoS.

3. L'estensione lato Client QoS invia una notifica degli eventi dei criteri QoS per il modulo di ispezione di QoS.

4. Il modulo di ispezione QoS recupera i criteri QoS di utente o computer e li archivia.

Quando un nuovo endpoint a livello di trasporto \(TCP connessione o il traffico UDP\) viene creato, viene eseguita questa procedura.

1. Il componente di livello di trasporto dello stack TCP/IP informa il modulo di ispezione di QoS.

2. Il modulo di ispezione QoS confronta i parametri dell'endpoint del livello di trasporto per i criteri QoS archiviati.

3. Se viene trovata una corrispondenza, il modulo di ispezione QoS contatta Pacer. sys per creare un flusso, una struttura di data che contiene il valore di DSCP e il traffico di limitazione delle richieste delle impostazioni dei criteri QoS di corrispondenza. Se sono presenti più criteri di QoS che corrispondono ai parametri dell'endpoint del livello di trasporto, viene usato il criterio di QoS più specifico.

4. Pacer. sys archivia il flusso e restituisce un numero di flusso corrispondente per il flusso per il modulo di ispezione di QoS.

5. Il modulo di ispezione QoS restituisce il numero di flusso a livello di trasporto.

6. Il livello di trasporto archivia il numero di flusso con l'endpoint a livello di trasporto.

Quando un pacchetto corrispondente a un endpoint a livello di trasporto è contrassegnata con un numero di flusso viene inviata, viene eseguita questa procedura.

1. Internamente, il livello di trasporto contrassegna il pacchetto con il numero di flusso.

2. Il livello di rete esegue una query Pacer. sys per il valore DSCP corrispondente al numero di flusso del pacchetto.

3. Pacer. sys restituisce il valore di DSCP per il livello di rete.

4. Il livello di rete consente di ottenere il valore di DSCP specificato da Pacer campo TOS IPv4 o campo della classe di traffico IPv6 e, per i pacchetti IPv4, calcola il checksum di intestazione IPv4 finale.

5. Il livello di rete inoltra il pacchetto a livello di Framing.

6. Poiché il pacchetto è stato contrassegnato con un numero di flusso, il livello di Framing trasferisce il pacchetto al Pacer. sys mediante NDIS 6.x.

7. Pacer. sys Usa il numero di flusso del pacchetto per determinare se il pacchetto deve essere limitato e in tal caso, consente di pianificare il pacchetto per l'invio.

8. Pacer. sys inoltra il pacchetto sia immediatamente \(se non viene rilevato traffico limitazione\) o come pianificato \(se il traffico è la limitazione delle richieste\) per NDIS 6.x per la trasmissione tramite la scheda di rete appropriata.

Questi processi di QoS basata su criteri offrono i vantaggi seguenti.

- Endpoint di livello per ogni trasporto, anziché ogni singolo pacchetto, viene eseguita l'ispezione del traffico per determinare se un criterio QoS viene applicato.

- Non ha alcun impatto sulle prestazioni per il traffico che corrisponde a un criterio QoS.

- Le applicazioni non sono necessario modificarla per sfruttare i vantaggi del servizio differenziato DSCP o la limitazione del traffico.

- I criteri QoS possono applicare al traffico protetto con IPsec.

Per l'argomento successivo in questa Guida, vedere [architettura dei criteri QoS](qos-policy-architecture.md).

Per il primo argomento in questa Guida, vedere [dei criteri di qualità del servizio (QoS)](qos-policy-top.md).
