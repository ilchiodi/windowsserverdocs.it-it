---
title: pathping
description: Informazioni su come ottenere la latenza di rete e le informazioni sulle perdite usando il comando pathping.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ec430125-b1dc-4aad-a7c9-b70f486d9e3c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 05675e4764c4c3e135647a1185da05634ee7264f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723365"
---
# <a name="pathping"></a>pathping

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Fornisce informazioni sulla latenza di rete e la perdita di rete a hop intermedi tra un'origine e una destinazione. **PathPing** invia più messaggi di richiesta echo a ogni router tra un'origine e una destinazione in un determinato periodo di tempo e calcola quindi i risultati in base ai pacchetti restituiti da ogni router. Poiché **PathPing** Visualizza il grado di perdita di pacchetti in un determinato router o collegamento, è possibile determinare quali router o subnet potrebbero riscontrare problemi di rete. 

**PathPing** esegue l'equivalente del comando **tracert** identificando i router che si trovano sul percorso. Invia quindi i ping periodicamente a tutti i router in un periodo di tempo specificato e calcola le statistiche in base al numero restituito da ciascuno. Usato senza parametri, **PathPing** Visualizza la guida. 

## <a name="syntax"></a>Sintassi
```
pathping [/n] [/h] [/g <Hostlist>] [/p <Period>] [/q <NumQueries> [/w <timeout>] [/i <IPaddress>] [/4 <IPv4>] [/6 <IPv6>][<TargetName>]
```
#### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/n|Impedisce a **PathPing** di tentare di risolvere gli indirizzi IP dei router intermedi con i rispettivi nomi. Questo può accelerare la visualizzazione dei risultati di **PathPing** .|
|/h \<MaximumHops>|Specifica il numero massimo di hop nel percorso in cui cercare la destinazione (destinazione). Il valore predefinito è 30 hop.|
|> \<di host/g|Specifica che i messaggi di richiesta echo usano l'opzione Loose Source Route nell'intestazione IP con il set di destinazioni intermedie specificato in *host*. Con il routing del codice sorgente sciolto, le destinazioni intermedie successive possono essere separate da uno o più router. Il numero massimo di indirizzi o nomi nell'elenco host è 9. L' *host* è una serie di indirizzi IP (in notazione decimale tratteggiata) separati da spazi.|
|/p \<> periodo|Specifica il numero di millisecondi di attesa tra i ping consecutivi. Il valore predefinito è 250 millisecondi (1/4 secondi).|
|/q \<NumQueries>|Specifica il numero di messaggi di richiesta echo inviati a ogni router nel percorso. Il valore predefinito è 100 query.|
|> \<timeout/w|Specifica il numero di millisecondi di attesa per ogni risposta. Il valore predefinito è 3000 millisecondi (3 secondi).|
|> \<IPAddress/i|Specifica l'indirizzo di origine.|
|/4 \<IPv4>|Specifica che pathping utilizza solo IPv4.|
|> IPv6 \</6|Specifica che pathping utilizza solo IPv6.|
|\<TargetName>|Specifica la destinazione, che viene identificata in base all'indirizzo IP o al nome host.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni
-   i parametri **PathPing** distinguono tra maiuscole e minuscole.
-   Per evitare congestioni di rete, i ping devono essere inviati a ritmi sufficientemente lenti.
-   Per ridurre al minimo gli effetti delle perdite di picchi, non inviare i ping troppo spesso.
-   Quando si usa il **/p** parametro, i ping vengono inviati singolarmente a ogni hop intermedio. Per questo motivo, l'intervallo tra due ping inviati allo stesso hop è il *periodo* moltiplicato per il numero di hop.
-   Quando si usa il parametro **/w** , è possibile inviare più ping in parallelo. Per questo motivo, la quantità di tempo specificata nel parametro *timeout* non è vincolata dalla quantità di tempo specificata nel parametro *period* per l'attesa tra i ping.
-   Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.

## <a name="examples"></a>Esempi

Per visualizzare l'output del comando **PathPing** :

```
D:\>pathping /n corp1
Tracing route to corp1 [10.54.1.196]
over a maximum of 30 hops:
  0  172.16.87.35
  1  172.16.87.218
  2  192.168.52.1
  3  192.168.80.1
  4  10.54.247.14
  5  10.54.1.196
computing statistics for 125 seconds...
            Source to Here   This Node/Link
Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  address
  0                                           172.16.87.35
                                0/ 100 =  0%   |
  1   41ms     0/ 100 =  0%     0/ 100 =  0%  172.16.87.218
                               13/ 100 = 13%   |
  2   22ms    16/ 100 = 16%     3/ 100 =  3%  192.168.52.1
                                0/ 100 =  0%   |
  3   24ms    13/ 100 = 13%     0/ 100 =  0%  192.168.80.1
                                0/ 100 =  0%   |
  4   21ms    14/ 100 = 14%     1/ 100 =  1%  10.54.247.14
                                0/ 100 =  0%   |
  5   24ms    13/ 100 = 13%     0/ 100 =  0%  10.54.1.196
Trace complete.
```
Quando **PathPing** viene eseguito, i primi risultati elencano il percorso. Si tratta dello stesso percorso visualizzato con il comando **tracert** . Viene quindi visualizzato un messaggio occupato per circa 90 secondi (l'ora varia in base al numero di hop). Durante questo periodo di tempo, le informazioni vengono raccolte da tutti i router elencati in precedenza e dai collegamenti tra di essi. al termine di questo periodo, vengono visualizzati i risultati del test.

Nel report di esempio precedente, le colonne **nodo/collegamento**, **perse/inviate = PCT** e **Indirizzo** indicano che il collegamento tra 172.16.87.218 e 192.168.52.1 sta eliminando il 13% dei pacchetti. Anche i router a hop 2 e 4 rilasciano i pacchetti a loro destinati, ma questa perdita non influisce sulla capacità di inoltrare il traffico non indirizzato a loro.

Il tasso di perdita visualizzato per i collegamenti, identificato come barra verticale (**|**) nella colonna **Address** , indica la congestione dei collegamenti che causa la perdita di pacchetti inoltrati nel percorso. I tassi di perdita visualizzati per i router (identificati dai rispettivi indirizzi IP) indicano che questi router potrebbero essere sottoposte a overload.

## <a name="additional-references"></a>Altri riferimenti
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)