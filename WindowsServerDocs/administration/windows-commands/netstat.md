---
title: netstat
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60e2718f-93cc-4ceb-bf0e-58a6a6e4fc8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c06684eb73639e7480b5bad39d4d679739682800
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437151"
---
# <a name="netstat"></a>netstat

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza le connessioni TCP attive, le porte su cui il computer è in ascolto, Ethernet statistiche, la tabella di routing IP, le statistiche IPv4 (per i protocolli IP ICMP, TCP e UDP) e le statistiche di IPv6 (per IPv6, ICMPv6, su IPv6 TCP e UDP su protocolli IPv6). Se utilizzato senza parametri, **netstat** Visualizza le connessioni TCP attive. 

## <a name="syntax"></a>Sintassi
```
netstat [-a] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<Interval>]
```

### <a name="parameters"></a>Parametri

|   Parametro   |                                                                                                                                              Descrizione                                                                                                                                              |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      -a       |                                                                                                   Visualizza tutte le connessioni TCP attive e le porte TCP e UDP su cui è in attesa il computer.                                                                                                   |
|      -e       |                                                                                 Visualizza statistiche Ethernet, ad esempio il numero di byte e pacchetti inviati e ricevuti. Questo parametro può essere combinato con **-s**.                                                                                  |
|      -n       |                                                                               Consente di visualizzare le connessioni TCP attive, tuttavia, indirizzi e numeri di porta sono espressi in ordine numerico e viene eseguito alcun tentativo per determinare i nomi.                                                                               |
|      -o       |                          Visualizza le connessioni TCP attive e include l'ID processo (PID) per ogni connessione. È possibile trovare l'applicazione in base al PID nella scheda processi in Gestione attività Windows. Questo parametro può essere combinato con **- un**, **- n**, e **-p**.                           |
| -p <Protocol> |               Mostra le connessioni per il protocollo specificato da *protocollo*. In questo caso, il *protocollo* può essere tcp, udp, tcpv6 o udpv6. Se questo parametro viene usato con **-s** per visualizzare le statistiche dal protocollo *protocollo* può essere tcp, udp, icmp, ip, tcpv6, udpv6, icmpv6 o ipv6.                |
|      -s       | Visualizza le statistiche dal protocollo. Per impostazione predefinita, vengono visualizzate le statistiche per i protocolli TCP, UDP, ICMP e IP. Se è installato il protocollo IPv6, vengono visualizzate le statistiche per il protocollo TCP su IPv6, UDP tramite IPv6 e ICMPv6 IPv6 protocolli. Il **-p** parametro può essere usato per specificare un set di protocolli. |
|      -r       |                                                                                                     Visualizza il contenuto della tabella di routing IP. Ciò equivale al comando di stampa di route.                                                                                                     |
|  <Interval>   |                                                        Viene visualizzata nuovamente le informazioni selezionate ogni *intervallo* secondi. Premere CTRL + C per interrompere la visualizzazione. Se questo parametro viene omesso, **netstat** viene stampata una sola volta le informazioni selezionate.                                                         |
|      /?       |                                                                                                                                 Visualizza la guida al prompt dei comandi.                                                                                                                                  |

## <a name="remarks"></a>Note
-   I parametri usati con questo comando devono essere preceduti da un segno meno ( **-** ) invece di una barra ( **/** ).
-   **netstat** fornisce statistiche per le operazioni seguenti:
    -   Proto il nome del protocollo (TCP o UDP).
    -   Indirizzo locale l'indirizzo IP del computer locale e il numero di porta in uso. Il nome del computer locale che corrisponde all'indirizzo IP e il nome della porta viene visualizzati a meno che il **- n** parametro è specificato. Se la porta non è ancora stabilita, il numero di porta viene visualizzato come un asterisco (*).
    -   indirizzo esterna IP indirizzo e il numero porta del computer remoto a cui è connesso il socket. I nomi che corrisponde all'indirizzo IP e la porta sono visualizzati, a meno che il **- n** parametro è specificato. Se la porta non è ancora stabilita, il numero di porta viene visualizzato come un asterisco (*).
    -   (stato) Indica lo stato di una connessione TCP. Gli stati possibili sono i seguenti: CLOSE_WAIT chiuso stabilita FIN_WAIT_1 FIN_WAIT_2 LAST_ACK listEN SYN_RECEIVED SYN_SEND timeD_WAIT per altre informazioni sugli stati di una connessione TCP, vedere la specifica Rfc 793.
-   Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.

## <a name="BKMK_Examples"></a>Esempi
Per visualizzare le statistiche di Ethernet sia le statistiche per tutti i protocolli, digitare:
```
netstat -e -s
```
Per visualizzare le statistiche per solo i protocolli TCP e UDP, digitare:
```
netstat -s -p tcp udp
```
Per visualizzare le connessioni TCP attive e gli ID di processo ogni 5 secondi, digitare:
```
netstat -o 5
```
Per visualizzare TCP attive le connessioni e il processo di ID in un formato numerico, digitare:
```
netstat -n -o
```

## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
