---
title: pathping
description: Informazioni su come ottenere la latenza di rete e le informazioni di perdita usando il comando pathping.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec430125-b1dc-4aad-a7c9-b70f486d9e3c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: db3a0f5cd3c711f7df0a13627969dc7b74b3d605
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837242"
---
# <a name="pathping"></a>pathping

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vengono fornite informazioni sulla latenza di rete e perdita di rete del hop intermedi tra origine e destinazione. **pathping** invia i messaggi di richiesta echo più a ogni router presente tra un'origine e destinazione in un periodo di tempo e calcola quindi i risultati in base ai pacchetti restituiti da ogni router. In quanto **pathping** consente di visualizzare il livello di perdita di pacchetti in un dato router o collegamento, è possibile determinare quali router o subnet potrebbero essersi verificati problemi di rete. 

**pathping** esegue l'equivalente del **tracert** comando identificando quali router sono nel percorso. Invia ping periodicamente a tutti i router in un periodo di tempo specificato e quindi calcola le statistiche in base al numero restituito da ognuno. Se utilizzato senza parametri, **pathping** Visualizza la Guida. 

## <a name="syntax"></a>Sintassi
```
pathping [/n] [/h] [/g <Hostlist>] [/p <Period>] [/q <NumQueries> [/w <timeout>] [/i <IPaddress>] [/4 <IPv4>] [/6 <IPv6>][<TargetName>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/n|Impedisce **pathping** dal tentativo di risolvere gli indirizzi IP dei router intermedi nei rispettivi nomi. Ciò potrebbe velocizzare la visualizzazione delle **pathping** risultati.|
|/h \<MaximumHops >|Specifica il numero massimo di hop nel percorso di ricerca per la destinazione (destinazione). Il valore predefinito è 30 hop.|
|/g \<Hostlist>|Specifica che l'utilizzo di messaggi di richiesta l'instradamento eco opzione nell'intestazione IP con il set di destinazioni intermediate specificato in *Hostlist*. Con il routing di origine separati, le destinazioni intermedie successive possono essere separate da uno o più router. Il numero massimo di indirizzi o nomi nell'elenco host è 9. Il *Hostlist* è una serie di indirizzi IP (in notazione decimale puntata) separata da spazi.|
|/p \<Period>|Specifica il numero di millisecondi di attesa tra i ping consecutivi. Il valore predefinito è 250 millisecondi (1 e 4 secondi).|
|/q \<NumQueries>|Specifica il numero di echo i messaggi inviati a ogni router nel percorso di richiesta. Il valore predefinito è 100 query.|
|/w \<timeout>|Specifica il numero di millisecondi di attesa per ogni risposta. Il valore predefinito è 3000 millisecondi (3 secondi).|
|/i \<IPaddress>|Specifica l'indirizzo di origine.|
|/4 \<IPv4>|Specifica che pathping utilizza solo IPv4.|
|/6 \<IPv6>|Specifica che pathping utilizza solo IPv6.|
|\<TargetName >|Specifica la destinazione, che è identificato dal nome host o indirizzo IP.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   **pathping** i parametri sono tra maiuscole e minuscole.
-   Per evitare la congestione della rete, i comandi ping devono essere inviati a una velocità sufficientemente lenta.
-   Per ridurre al minimo gli effetti di eventuali perdite di burst, non inviare ping troppo frequentemente.
-   Quando si usa la **/p** parametro ping vengono inviati singolarmente per ogni hop intermedi. Per questo motivo, è l'intervallo tra i due ping inviati allo stesso hop *periodo* moltiplicato per il numero di hop.
-   Quando si usa la **/w** parametro, è possibile inviare più ping in parallelo. Per questo motivo, la quantità di tempo specificato nella *timeout* parametro non è vincolato dalla quantità di tempo specificato nel *periodo* parametro per l'attesa tra i ping.
-   Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.

## <a name="BKMK_Examples"></a>Esempi

L'esempio seguente illustra **pathping** output del comando:

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
Quando **pathping** viene eseguito, i primi risultati elencare il percorso. Questo è lo stesso percorso viene visualizzato utilizzando il **tracert** comando. Successivamente, viene visualizzato un messaggio occupato per circa 90 secondi (il tempo varia in base al conteggio hop). Durante questo periodo, le informazioni vengono raccolte da tutti i router elencati in precedenza e dai collegamenti tra di essi. alla fine di questo periodo, vengono visualizzati i risultati del test.

Nel report di esempio precedente, il **questo nodo/collegamento**, **Lost o Sent = Pct** e **indirizzo** colonne indicano che il collegamento tra 172.16.87.218 e 192.168.52.1 perde il 13 percentuale di pacchetti. Il router presso gli hop 2 e 4 Elimina anche i pacchetti indirizzati ad essi, ma questa perdita non influisce sulla loro capacità di inoltrare il traffico non indirizzato a loro.

I tassi di perdita visualizzati per i collegamenti, identificati come una barra verticale (**|**) nella **indirizzo** colonna indicano una congestione che sta causando la perdita di pacchetti che vengono inoltrati nel percorso. I tassi di perdita visualizzati per i router (identificati dai rispettivi indirizzi IP) indicano che potrebbero eseguire l'overload di questi router.

## <a name="additional-references"></a>Altri riferimenti
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)