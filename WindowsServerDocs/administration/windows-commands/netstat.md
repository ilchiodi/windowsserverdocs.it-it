---
title: netstat
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60e2718f-93cc-4ceb-bf0e-58a6a6e4fc8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afd34cca2ecd3caa7ac480b380b85ba6d2a19fcb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839014"
---
# <a name="netstat"></a>netstat

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza le connessioni TCP attive, le porte su cui il computer è in ascolto, le statistiche Ethernet, la tabella di routing IP, le statistiche IPv4 (per i protocolli IP, ICMP, TCP e UDP) e le statistiche IPv6 (per i protocolli IPv6, ICMPv6, TCP su IPv6 e UDP su IPv6). Usato senza parametri, **netstat** Visualizza le connessioni TCP attive. 

## <a name="syntax"></a>Sintassi
```
netstat [-a] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<Interval>]
```

#### <a name="parameters"></a>Parametri

|   Parametro   |                                                                                                                                              Descrizione                                                                                                                                              |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      -a       |                                                                                                   Consente di visualizzare tutte le connessioni TCP attive e le porte TCP e UDP su cui è in ascolto il computer.                                                                                                   |
|      -e       |                                                                                 Visualizza le statistiche Ethernet, ad esempio il numero di byte e i pacchetti inviati e ricevuti. Questo parametro può essere combinato con **-s**.                                                                                  |
|      -n       |                                                                               Visualizza le connessioni TCP attive, tuttavia, gli indirizzi e i numeri di porta vengono espressi numericamente e non viene effettuato alcun tentativo di determinare i nomi.                                                                               |
|      -o       |                          Visualizza le connessioni TCP attive e include l'ID processo (PID) per ogni connessione. È possibile trovare l'applicazione in base al PID nella scheda processi di gestione attività di Windows. Questo parametro può essere combinato con **-a**, **-n**e **-p**.                           |
| -p <Protocol> |               Mostra le connessioni per il protocollo specificato dal *protocollo*. In questo caso, il *protocollo* può essere TCP, UDP, TCPv6 o UDPv6. Se questo parametro viene utilizzato con **-s** per visualizzare le statistiche per protocollo, il *protocollo* può essere TCP, UDP, ICMP, IP, TCPv6, UDPv6, ICMPv6 o IPv6.                |
|      -s       | Visualizza le statistiche per protocollo. Per impostazione predefinita, vengono visualizzate le statistiche per i protocolli TCP, UDP, ICMP e IP. Se è installato il protocollo IPv6, vengono visualizzate le statistiche per i protocolli TCP su IPv6, UDP su IPv6, ICMPv6 e IPv6. Il parametro **-p** può essere utilizzato per specificare un set di protocolli. |
|      -r       |                                                                                                     Consente di visualizzare il contenuto della tabella di routing IP. Equivale al comando route print.                                                                                                     |
|  <Interval>   |                                                        Visualizza nuovamente le informazioni selezionate ogni *intervallo* di secondi. Premere CTRL + C per arrestare la rivisualizzazione. Se questo parametro viene omesso, **netstat** stampa le informazioni selezionate una sola volta.                                                         |
|      /?       |                                                                                                                                 Visualizza la guida al prompt dei comandi.                                                                                                                                  |

## <a name="remarks"></a>Note
-   I parametri usati con questo comando devono essere preceduti da un trattino ( **-** ) anziché da una barra ( **/** ).
-   **netstat** fornisce le statistiche per gli elementi seguenti:
    -   Proto nome del protocollo (TCP o UDP).
    -   Indirizzo locale l'indirizzo IP del computer locale e il numero di porta utilizzato. Il nome del computer locale che corrisponde all'indirizzo IP e il nome della porta viene visualizzato a meno che non sia specificato il parametro **-n** . Se la porta non è ancora stata stabilita, il numero di porta viene visualizzato come asterisco (*).
    -   indirizzo esterno numero di porta e indirizzo IP del computer remoto a cui è connesso il socket. I nomi che corrispondono all'indirizzo IP e alla porta vengono visualizzati a meno che non sia specificato il parametro **-n** . Se la porta non è ancora stata stabilita, il numero di porta viene visualizzato come asterisco (*).
    -   stato Indica lo stato di una connessione TCP. Gli stati possibili sono i seguenti: CLOSE_WAIT CLOSED ESTABLISHed FIN_WAIT_1 FIN_WAIT_2 LAST_ACK listEN SYN_RECEIVED SYN_SEND timeD_WAIT per ulteriori informazioni sugli stati di una connessione TCP, vedere RFC 793.
-   Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi
Per visualizzare le statistiche Ethernet e le statistiche per tutti i protocolli, digitare:
```
netstat -e -s
```
Per visualizzare le statistiche solo per i protocolli TCP e UDP, digitare:
```
netstat -s -p tcp udp
```
Per visualizzare le connessioni TCP attive e gli ID processo ogni 5 secondi, digitare:
```
netstat -o 5
```
Per visualizzare le connessioni TCP attive e gli ID processo utilizzando il formato numerico, digitare:
```
netstat -n -o
```

## <a name="additional-references"></a>Altre informazioni di riferimento
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
