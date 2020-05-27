---
title: tracert
description: Argomento di riferimento per tracert, che determina il percorso effettuato a una destinazione, inviando Internet Control Message Protocol (ICMP) Echo Requests o messaggi ICMPv6 alla destinazione con valori di campo time to Live (TTL) a incremento incrementale.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9032a032-2e5e-49d4-9e86-f821600e4ba6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25dafb691513eae38dd99aef6288ee1bfb2d80be
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821401"
---
# <a name="tracert"></a>tracert

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Determina il percorso effettuato a una destinazione inviando i messaggi di richiesta echo Internet Control Message Protocol (ICMP) o ICMPv6 alla destinazione con valori di campo TTL (time to Live) che aumentano in modo incrementale. Il percorso visualizzato è l'elenco di interfacce router vicino/laterali dei router nel percorso tra un host di origine e una destinazione. L'interfaccia near/Side è l'interfaccia del router più vicina all'host di invio nel percorso. Utilizzato senza parametri, tracert Visualizza la guida.

## <a name="syntax"></a>Sintassi
```
tracert [/d] [/h <MaximumHops>] [/j <Hostlist>] [/w <timeout>] [/R] [/S <Srcaddr>] [/4][/6] <TargetName>
```
#### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/d|Impedisce a **tracert** di tentare di risolvere gli indirizzi IP dei router intermedi ai rispettivi nomi. In questo modo è possibile velocizzare la visualizzazione dei risultati di **tracert** .|
|/h \< MaximumHops>|Specifica il numero massimo di hop nel percorso in cui cercare la destinazione (destinazione). Il valore predefinito è 30 hop.|
|\<> host/j|Specifica che i messaggi di richiesta echo usano l'opzione Loose Source Route nell'intestazione IP con il set di destinazioni intermedie specificato in *host*. Con il routing del codice sorgente sciolto, le destinazioni intermedie successive possono essere separate da uno o più router. Il numero massimo di indirizzi o nomi nell'elenco host è 9. L' *host* è una serie di indirizzi IP (in notazione decimale tratteggiata) separati da spazi. Utilizzare questo parametro solo quando si tracciano gli indirizzi IPv4.|
|\<> timeout/w|Specifica la quantità di tempo in millisecondi di attesa del superamento del tempo ICMP o del messaggio di risposta echo corrispondente a un determinato messaggio di richiesta echo da ricevere. Se non viene ricevuto entro il timeout, viene visualizzato un asterisco (*). Il timeout predefinito è 4000 (4 secondi).|
|/R|Specifica che l'intestazione dell'estensione di routing IPv6 deve essere utilizzata per inviare un messaggio di richiesta echo all'host locale, utilizzando la destinazione come destinazione intermedia e testando la route inversa.|
|/S \< Srcaddr>|Specifica l'indirizzo di origine da utilizzare nei messaggi di richiesta echo. Utilizzare questo parametro solo quando si tracciano indirizzi IPv6.|
|/4|Specifica che tracert. exe può utilizzare solo IPv4 per questa traccia.|
|/6|Specifica che tracert. exe può utilizzare solo IPv6 per questa traccia.|
|\<TargetName>|Specifica la destinazione, identificata in base all'indirizzo IP o al nome host.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni
-   Questo strumento di diagnostica determina il percorso effettuato a una destinazione inviando i messaggi di richiesta echo ICMP con valori TTL (time to Live) diversi alla destinazione. Ogni router lungo il percorso è necessario per decrementare la durata (TTL) in un pacchetto IP di almeno 1 prima di procedere con l'invio. In realtà, la durata (TTL) è un contatore di collegamenti massimo. Quando la durata (TTL) di un pacchetto raggiunge 0, il router deve restituire al computer di origine un messaggio che supera il tempo ICMP. tracert determina il percorso inviando il primo messaggio di richiesta echo con un valore TTL 1 e incrementando la durata (TTL) di 1 a ogni trasmissione successiva finché la destinazione non risponde o viene raggiunto il numero massimo di hop. Il numero massimo di hop è 30 per impostazione predefinita e può essere specificato utilizzando il parametro **/h** . Il percorso è determinato esaminando il tempo ICMP che ha superato i messaggi restituiti dai router intermedi e il messaggio di risposta echo restituito dalla destinazione. Alcuni router, tuttavia, non restituiscono tempi superiori ai messaggi per i pacchetti con valori TTL scaduti e invisile al comando tracert. In questo caso, viene visualizzata una riga di asterischi (*) per tale hop.
-   Per tracciare un percorso e fornire la latenza di rete e la perdita di pacchetti per ogni router e collegamento nel percorso, usare il comando **PathPing** .
-   Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.

## <a name="examples"></a>Esempi
Per tracciare il percorso dell'host denominato corp7.microsoft.com, digitare:
```
tracert corp7.microsoft.com
```
Per tracciare il percorso dell'host denominato corp7.microsoft.com e impedire la risoluzione di ogni indirizzo IP sul nome, digitare:
```
tracert /d corp7.microsoft.com
```
Per tracciare il percorso dell'host denominato corp7.microsoft.com e usare il percorso di origine libero 10.12.0.1/10.29.3.1/10.1.44.1, digitare:
```
tracert /j 10.12.0.1 10.29.3.1 10.1.44.1 corp7.microsoft.com
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
