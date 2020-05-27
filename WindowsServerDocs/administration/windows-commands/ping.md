---
title: ping
description: Usare il comando ping per verificare la connettività di rete.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 49272671-2eec-4fa5-881f-65c24cfbef52
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6e97ed800ab64a9a7ec276ed6bb498ef04139ca6
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820771"
---
# <a name="ping"></a>ping

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il comando **ping** verifica la connettività a livello IP a un altro computer TCP/IP inviando messaggi di richiesta echo di Internet Control Message Protocol (ICMP). Viene visualizzata la ricezione dei messaggi di risposta echo corrispondenti, insieme ai tempi di round trip. ping è il comando TCP/IP primario usato per risolvere i problemi di connettività, raggiungibilità e risoluzione dei nomi. Usato senza parametri, **ping** Visualizza la guida.

## <a name="syntax"></a>Sintassi

```
ping [/t] [/a] [/n <Count>] [/l <Size>] [/f] [/I <TTL>] [/v <TOS>] [/r <Count>] [/s <Count>] [{/j <Hostlist> | /k <Hostlist>}] [/w <timeout>] [/R] [/S <Srcaddr>] [/4] [/6] <TargetName>
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|/t|Specifica che il ping continua a inviare messaggi di richiesta echo alla destinazione fino a quando non viene interrotto. Per interrompere e visualizzare le statistiche, premere CTRL + INTERR. Per interrompere e chiudere **ping**, premere CTRL + C.|
|/a|Specifica che la risoluzione dei nomi inversi viene eseguita sull'indirizzo IP di destinazione. Se l'operazione ha esito positivo, ping Visualizza il nome host corrispondente.|
|/n \< conteggio\>|Specifica il numero di messaggi di richiesta echo inviati. Il valore predefinito è 4.|
|\<dimensioni/l\>|Specifica la lunghezza, in byte, del campo dati nei messaggi di richiesta echo inviati. Il valore predefinito è 32. La dimensione massima è 65.527.|
|/f|Specifica che i messaggi di richiesta echo vengono inviati con il flag do not Fragment nell'intestazione IP impostata su 1 (disponibile solo in IPv4). Il messaggio di richiesta echo non può essere frammentato da router nel percorso della destinazione. Questo parametro è utile per la risoluzione dei problemi PMTU (Maximum Transmission Unit) del percorso.|
|/i \< TTL\>|Specifica il valore del campo TTL nell'intestazione IP per i messaggi di richiesta echo inviati. Il valore predefinito è il valore TTL predefinito per l'host. Il valore *TTL* massimo è 255.|
|/v \< TOS\>|Specifica il valore del campo tipo di servizio (TOS) nell'intestazione IP per i messaggi di richiesta echo inviati (disponibile solo in IPv4). Il valore predefinito è 0. *TOS* viene specificato come valore decimale compreso tra 0 e 255.|
|\<conteggio/r\>|Specifica che l'opzione Registra route nell'intestazione IP viene utilizzata per registrare il percorso eseguito dal messaggio di richiesta echo e il messaggio di risposta echo corrispondente (disponibile solo in IPv4). Ogni hop nel percorso usa una voce nell'opzione Registra route. Se possibile, specificare un *conteggio* uguale o maggiore del numero di hop tra l'origine e la destinazione. Il *conteggio* deve essere un minimo di 1 e un massimo di 9.|
|\<numero/s\>|Specifica che l'opzione timestamp Internet nell'intestazione IP viene utilizzata per registrare l'ora di arrivo per il messaggio di richiesta echo e il messaggio di risposta echo corrispondente per ogni hop. Il *conteggio* deve essere un minimo di 1 e un massimo di 4. Questa operazione è necessaria per gli indirizzi di destinazione locali per il collegamento.|
|\<host/j\>|Specifica che i messaggi di richiesta echo utilizzano l'opzione Loose Source Route nell'intestazione IP con il set di destinazioni intermedie specificato in *host* , disponibile solo in IPv4. Con il routing del codice sorgente sciolto, le destinazioni intermedie successive possono essere separate da uno o più router. Il numero massimo di indirizzi o nomi nell'elenco host è 9. L'elenco host è costituito da una serie di indirizzi IP (in notazione decimale tratteggiata) separati da spazi.|
|\<host/k\>|Specifica che i messaggi di richiesta echo utilizzano l'opzione Strict Source Route nell'intestazione IP con il set di destinazioni intermedie specificato in *host* , disponibile solo in IPv4. Con il routing del codice sorgente rigoroso, la destinazione intermedia successiva deve essere direttamente raggiungibile (deve essere un router adiacente in un'interfaccia del router). Il numero massimo di indirizzi o nomi nell'elenco host è 9. L'elenco host è costituito da una serie di indirizzi IP (in notazione decimale tratteggiata) separati da spazi.|
|\<timeout/w\>|Specifica la quantità di tempo, in millisecondi, di attesa per la ricezione del messaggio di risposta echo corrispondente a un determinato messaggio di richiesta echo. Se il messaggio di risposta echo non viene ricevuto entro il timeout, viene visualizzato il messaggio di errore "timeout della richiesta". Il timeout predefinito è 4000 (4 secondi).|
|/R|Specifica che il percorso di round trip viene tracciato (disponibile solo su IPv6).|
|/S \< srcaddr\>|Specifica l'indirizzo di origine da utilizzare (disponibile solo su IPv6).|
|/4|Specifica che viene utilizzato IPv4 per eseguire il ping. Questo parametro non è necessario per identificare l'host di destinazione con un indirizzo IPv4. È necessario solo per identificare l'host di destinazione in base al nome.|
|/6|Specifica che IPv6 viene utilizzato per eseguire il ping. Questo parametro non è necessario per identificare l'host di destinazione con un indirizzo IPv6. È necessario solo per identificare l'host di destinazione in base al nome.|
|\<TargetName\>|Specifica il nome host o l'indirizzo IP della destinazione.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   È possibile utilizzare il comando **ping** per testare il nome del computer e l'indirizzo IP del computer. Se il ping dell'indirizzo IP ha esito positivo, ma il ping del nome del computer non lo è, potrebbe essere presente un problema di risoluzione dei nomi. In tal caso, assicurarsi che il nome del computer che si sta specificando possa essere risolto tramite il file hosts locale, usando le query Domain Name System (DNS) o tramite le tecniche di risoluzione dei nomi NetBIOS.
-   Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.

## <a name="examples"></a>Esempi

Per visualizzare l'output del comando **ping** :

```
C:\>ping example.microsoft.com
         pinging example.microsoft.com [192.168.239.132] with 32 bytes of data:
         Reply from 192.168.239.132: bytes=32 time=101ms TTL=124
         Reply from 192.168.239.132: bytes=32 time=100ms TTL=124
         Reply from 192.168.239.132: bytes=32 time=120ms TTL=124
         Reply from 192.168.239.132: bytes=32 time=120ms TTL=124
```

Per effettuare il ping della 10.0.99.221 di destinazione e risolvere 10.0.99.221 nel nome host, digitare:

```
ping /a 10.0.99.221
```

Per effettuare il ping della 10.0.99.221 di destinazione con 10 messaggi di richiesta echo, ognuno dei quali ha un campo dati di 1000 byte, digitare:

```
ping /n 10 /l 1000 10.0.99.221
```

Per effettuare il ping della 10.0.99.221 di destinazione e registrare la route per 4 hop, digitare:

```
ping /r 4 10.0.99.221
```

Per effettuare il ping della 10.0.99.221 di destinazione e specificare la route di origine separata di 10.12.0.1-10.29.3.1-10.1.44.1, digitare:

```
ping /j 10.12.0.1 10.29.3.1 10.1.44.1 10.0.99.221
```

## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
