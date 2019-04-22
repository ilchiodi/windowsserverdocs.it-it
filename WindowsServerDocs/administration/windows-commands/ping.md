---
title: ping
description: Usare il comando ping per verificare la connettività di rete.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 49272671-2eec-4fa5-881f-65c24cfbef52
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 1ac02a148061cd6eb8480c67f15e934f5fd57768
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816752"
---
# <a name="ping"></a>ping

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il **ping** comando verifica la connettività a livello IP a un altro computer TCP/IP inviando messaggi di richiesta echo di Internet controllo Message Protocol (ICMP). La ricezione del corrispondente echo Reply vengono visualizzati i messaggi, insieme ai tempi di round trip. ping è il comando principale di TCP/IP utilizzato per risolvere i problemi di connettività, raggiungibilità e la risoluzione dei nomi. Se utilizzato senza parametri, **ping** Visualizza la Guida.

## <a name="syntax"></a>Sintassi

```
ping [/t] [/a] [/n <Count>] [/l <Size>] [/f] [/I <TTL>] [/v <TOS>] [/r <Count>] [/s <Count>] [{/j <Hostlist> | /k <Hostlist>}] [/w <timeout>] [/R] [/S <Srcaddr>] [/4] [/6] <TargetName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|/t|Specifica che il ping di continuare l'invio di messaggi richiesta echo nella destinazione fino a quando non interrotta. Per interrompere e visualizzare le statistiche, premere CTRL + INTERR. Per interrompere ed Esci **ping**, premere CTRL + C.|
|/a|Specifica che la risoluzione dei nomi inversa viene eseguita sull'indirizzo IP di destinazione. Se l'operazione viene completata, il comando ping consente di visualizzare il nome host corrispondente.|
|/n \<conteggio\>|Specifica il numero di echo inviati i messaggi di richiesta. Il valore predefinito è 4.|
|/l \<dimensioni\>|Specifica la lunghezza, espressa in byte, del campo dati nell'eco i messaggi di richiesta inviati. Il valore predefinito è 32. La dimensione massima è 65.527.|
|/f|Specifica che eseguono l'eco dei messaggi di richiesta vengono inviati con il flag nell'intestazione IP impostato su 1 (disponibile solo su IPv4) di non frammentazione. Non è possibile frammentare il messaggio di richiesta echo dai router nel percorso di destinazione. Questo parametro è utile per la risoluzione dei problemi di percorso massima Transmission Unit rilevamento della PMTU ().|
|/I \<TTL\>|Specifica il valore del campo Durata (TTL) nell'intestazione IP per echo inviati i messaggi di richiesta. Il valore predefinito è il valore di durata (TTL) predefinito per l'host. Il valore massimo *TTL* è 255.|
|/v \<TOS\>|Specifica il valore del tipo di campo del servizio (TOS) nell'intestazione IP per echo Request i messaggi inviati (disponibili solo su IPv4). Il valore predefinito è 0. *TOS* viene specificato come valore decimale compreso tra 0 e 255.|
|/r \<conteggio\>|Specifica che l'opzione di Route di Record nell'intestazione IP viene usato per registrare il percorso per il messaggio di richiesta echo e corrispondente visualizzato questo messaggio di risposta (disponibile solo su IPv4). Ogni hop nel percorso Usa una voce in tale opzione. Se possibile, specificare una *conteggio* che è uguale o maggiore del numero di hop tra l'origine e destinazione. Il *conteggio* deve essere almeno pari a 1 e un massimo di 9.|
|/s \<conteggio\>|Specifica che l'opzione di timestamp Internet nell'intestazione IP viene utilizzato per registrare l'ora di arrivo per il messaggio di richiesta echo e corrispondente messaggio di risposta per ogni hop echo. Il *conteggio* deve essere almeno pari a 1 e un massimo di 4. Ciò è necessario per gli indirizzi di destinazione locali al collegamento.|
|/j \<Hostlist\>|Specifica che l'utilizzo di messaggi di richiesta l'instradamento eco opzione nell'intestazione IP con il set di destinazioni intermediate specificato in *Hostlist* (disponibile solo su IPv4). Con il routing di origine separati, le destinazioni intermedie successive possono essere separate da uno o più router. Il numero massimo di indirizzi o nomi nell'elenco host è 9. L'elenco di host è una serie di indirizzi IP (in notazione decimale puntata) separata da spazi.|
|/k \<Hostlist\>|Specifica che l'eco richiesta messaggi utilizza le Route di origine Strict opzione nell'intestazione IP con il set di destinazioni intermediate specificato in *Hostlist* (disponibile solo su IPv4). Con l'origine strict routing, la destinazione intermedia successiva deve essere raggiungibile direttamente (deve essere un elemento in un'interfaccia del router adiacente). Il numero massimo di indirizzi o nomi nell'elenco host è 9. L'elenco di host è una serie di indirizzi IP (in notazione decimale puntata) separata da spazi.|
|/w \<timeout\>|Specifica la quantità di tempo, espresso in millisecondi, per attendere l'eco del messaggio di risposta che corrisponde a un messaggio di richiesta echo specificato per la ricezione. Se il messaggio di risposta echo non viene ricevuta entro il timeout, viene visualizzato il messaggio di errore "Timeout della richiesta". Il timeout predefinito è 4000 (4 secondi).|
|/R|Specifica che il percorso di andata e ritorno viene tracciato (disponibile solo su IPv6).|
|/S \<Srcaddr\>|Specifica l'indirizzo di origine da utilizzare (disponibile solo su IPv6).|
|/4|Specifica che IPv4 viene utilizzato per effettuare il ping. Questo parametro non è necessario identificare l'host di destinazione con un indirizzo IPv4. È solo necessario per identificare l'host di destinazione in base al nome.|
|/6|Specifica che IPv6 viene utilizzato per effettuare il ping. Questo parametro non è necessario identificare l'host di destinazione con un indirizzo IPv6. È solo necessario per identificare l'host di destinazione in base al nome.|
|\<TargetName\>|Specifica il nome host o indirizzo IP di destinazione.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   È possibile usare **ping** da testare sia il nome del computer e l'indirizzo IP del computer. Se il ping l'indirizzo IP ha esito positivo, ma il ping del nome di computer non è, potrebbe essere un problema di risoluzione dei nomi. In questo caso, assicurarsi che il nome del computer che si specifica può essere risolto tramite il file Hosts locale, usando le query di sistema DNS (Domain Name), o tramite NetBIOS denominare le tecniche di risoluzione.
-   Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.

## <a name="BKMK_Examples"></a>Esempi

L'esempio seguente illustra **ping** output del comando:

```
C:\>ping example.microsoft.com       
         pinging example.microsoft.com [192.168.239.132] with 32 bytes of data:       
         Reply from 192.168.239.132: bytes=32 time=101ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=100ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=120ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=120ms TTL=124
```

Per eseguire il ping della destinazione 10.0.99.221 e risolvere 10.0.99.221 al nome host, digitare:

```
ping /a 10.0.99.221
```

Per eseguire il ping della destinazione 10.0.99.221 con messaggi di richiesta echo 10, ognuno dei quali ha un campo di dati di 1000 byte, digitare:

```
ping /n 10 /l 1000 10.0.99.221
```

Per eseguire il ping della destinazione 10.0.99.221 e registrare la route per 4 hop, digitare:

```
ping /r 4 10.0.99.221
```

Per eseguire il ping della destinazione 10.0.99.221 e specificare l'instradamento di 10.12.0.1-10.29.3.1-10.1.44.1, digitare:

```
ping /j 10.12.0.1 10.29.3.1 10.1.44.1 10.0.99.221
```

## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
