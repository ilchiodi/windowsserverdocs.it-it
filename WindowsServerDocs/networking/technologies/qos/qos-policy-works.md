---
title: Funzionamento dei criteri QoS
description: In questo argomento viene fornita una panoramica di qualità del servizio (QoS), che consente di usare criteri di gruppo per stabilire le priorità del traffico larghezza di banda di specifiche applicazioni e servizi in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1073308b5939e648fdcc2006acdce76ecf0331c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="how-qos-policy-works"></a>Funzionamento dei criteri QoS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Quando l'avvio o ottenere utente aggiornato o impostazioni di criteri di gruppo Configurazione computer per QoS, si verifica quanto segue.

1. Il motore dei criteri di gruppo consente di recuperare le impostazioni di criteri di gruppo Configurazione computer o utente da Active Directory.

2. Il motore dei criteri di gruppo informa l'estensione lato Client QoS che sono state eseguite modifiche nei criteri QoS.

3. L'estensione lato Client QoS invia una notifica di eventi di criteri QoS per il modulo di ispezione QoS.

4. Il modulo di ispezione QoS recupera i criteri QoS utente o computer e li archivia.

Quando un nuovo endpoint Layer di trasporto \ (connessione TCP o UDP traffic\) viene creato, si verifica il processo seguente.

1. Il componente Layer di trasporto dello stack TCP/IP informa il modulo di ispezione QoS.

2. Il modulo di ispezione QoS confronta i parametri dell'endpoint Layer di trasporto per i criteri di QoS di archiviazione.

3. Se viene trovata una corrispondenza, il modulo di ispezione QoS contatta Pacer.sys per creare un flusso di una struttura di dati che contiene il valore DSCP e il traffico di limitazione delle impostazioni del criterio di QoS corrispondente. Se sono presenti più criteri QoS che corrispondono ai parametri dell'endpoint Layer di trasporto, viene utilizzato il criterio QoS più specifico.

4. Pacer.sys archivia il flusso e restituisce un numero di flusso corrispondente al flusso per il modulo di ispezione QoS.

5. Il modulo di ispezione QoS restituisce il numero di flusso per il livello di trasporto.

6. Il Layer di trasporto memorizza il numero di flusso con l'endpoint Layer di trasporto.

Quando un pacchetto corrispondente a un endpoint Layer di trasporto contrassegnati con un numero di flusso viene inviato, si verifica il processo seguente.

1. Il Layer di trasporto contrassegna internamente il pacchetto con il numero di flusso.

2. Livello di rete esegue query Pacer.sys per il valore DSCP corrispondente al numero di flusso del pacchetto.

3. Pacer.sys restituisce il valore DSCP a livello di rete.

4. Livello di rete consente di ottenere il valore DSCP specificato dal Pacer.sys campo TOS IPv4 o campo della classe di traffico IPv6 e, per i pacchetti IPv4, calcola il checksum intestazione IPv4 finale.

5. Livello di rete passa il pacchetto al livello di Framing.

6. Poiché il pacchetto è stato contrassegnato con un numero di flusso, il livello di frame passa il pacchetto Pacer.sys tramite NDIS 6. x.

7. Pacer.sys Usa il numero di flusso del pacchetto per determinare se il pacchetto deve essere limitate e in tal caso, consente di pianificare il pacchetto per l'invio.

8. Pacer.sys pratico il pacchetto sia immediatamente \ (se è presente alcun throttling\ traffico) o come pianificato \ (se è presente il traffico throttling\) a NDIS 6. x per la trasmissione tramite la scheda di rete.

Questi processi di QoS basata su criteri forniscono i vantaggi seguenti.

- L'ispezione del traffico per determinare se si applica un criterio QoS viene effettuata endpoint livello al trasporto, anziché per ogni pacchetto.

- Non esiste alcun impatto sulle prestazioni per il traffico che corrisponde a un criterio QoS.

- Le applicazioni non è necessario essere modificate per sfruttare i vantaggi del servizio differenziato DSCP o limitazione del traffico.

- Applicano i criteri QoS a traffico protetto con IPsec.

Per l'argomento successivo in questa Guida, vedere [architettura dei criteri QoS](qos-policy-architecture.md).

Per il primo argomento in questa Guida, vedere [criteri Quality of Service (QoS)](qos-policy-top.md).
