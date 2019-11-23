---
title: Funzionamento dei criteri QoS
description: Questo argomento fornisce una panoramica dei criteri di qualità del servizio (QoS), che consente di usare Criteri di gruppo per assegnare priorità alla larghezza di banda del traffico di rete di applicazioni e servizi specifici in Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4de9674e2d1700d342af380c79a611c3d5961cda
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405175"
---
# <a name="how-qos-policy-works"></a>Funzionamento dei criteri QoS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Quando si avvia o si ottiene una configurazione utente o computer aggiornata Criteri di gruppo impostazioni per QoS, si verifica il processo seguente.

1. Il motore di Criteri di gruppo Recupera le impostazioni di configurazione dell'utente o del computer Criteri di gruppo da Active Directory.

2. Il motore di Criteri di gruppo informa l'estensione lato client QoS che sono state apportate modifiche ai criteri QoS.

3. L'estensione QoS lato client invia una notifica degli eventi dei criteri QoS al modulo di ispezione QoS.

4. Il modulo di ispezione QoS recupera i criteri QoS utente o computer e li archivia.

Quando viene creato un nuovo endpoint del livello di trasporto \(connessione TCP o il traffico UDP\), si verifica il processo seguente.

1. Il componente livello trasporto dello stack TCP/IP informa il modulo di ispezione QoS.

2. Il modulo di ispezione QoS Confronta i parametri dell'endpoint del livello di trasporto con i criteri QoS archiviati.

3. Se viene trovata una corrispondenza, il modulo di ispezione QoS Contatta Pacer. sys per creare un flusso, una struttura di dati che contiene il valore DSCP e le impostazioni di limitazione del traffico del criterio QoS corrispondente. Se sono presenti più criteri QoS che corrispondono ai parametri dell'endpoint del livello di trasporto, vengono usati i criteri QoS più specifici.

4. Pacer. sys archivia il flusso e restituisce un numero di flusso corrispondente al flusso al modulo di ispezione QoS.

5. Il modulo di ispezione QoS restituisce il numero di flusso al livello trasporto.

6. Il livello di trasporto archivia il numero di flusso con l'endpoint del livello di trasporto.

Quando viene inviato un pacchetto corrispondente a un endpoint del livello di trasporto contrassegnato con un numero di flusso, si verifica il processo seguente.

1. Il livello di trasporto contrassegna internamente il pacchetto con il numero di flusso.

2. Il livello di rete esegue una query su pacemaker. sys per il valore DSCP corrispondente al numero di flusso del pacchetto.

3. Pacer. sys restituisce il valore DSCP al livello di rete.

4. Il livello di rete modifica il campo IPv4 TOS o la classe di traffico IPv6 nel valore DSCP specificato da Pacer. sys e, per i pacchetti IPv4, calcola il checksum finale dell'intestazione IPv4.

5. Il livello di rete passa il pacchetto al livello di framing.

6. Poiché il pacchetto è stato contrassegnato con un numero di flusso, il livello di framing passa il pacchetto a Pacer. sys tramite NDIS 6. x.

7. Pacer. sys usa il numero di flusso del pacchetto per determinare se il pacchetto deve essere limitato e, in tal caso, Pianifica il pacchetto per l'invio.

8. Pacer. sys consente di passare immediatamente al pacchetto \(se non è presente alcuna limitazione del traffico\) o come \(pianificata se è presente una limitazione del traffico\) a NDIS 6. x per la trasmissione tramite la scheda di rete appropriata.

Questi processi di QoS basata su criteri offrono i vantaggi seguenti.

- L'ispezione del traffico per determinare se si applica un criterio QoS viene eseguita per endpoint del livello di trasporto, anziché per pacchetto.

- Non vi è alcun effetto sulle prestazioni per il traffico che non corrisponde a un criterio QoS.

- Non è necessario modificare le applicazioni per sfruttare la limitazione del traffico o del servizio differenziato basato su DSCP.

- I criteri QoS possono essere applicati al traffico protetto con IPsec.

Per l'argomento successivo di questa guida, vedere l'articolo relativo all' [architettura dei criteri QoS](qos-policy-architecture.md).

Per il primo argomento di questa guida, vedere [criteri di qualità del servizio (QoS)](qos-policy-top.md).
