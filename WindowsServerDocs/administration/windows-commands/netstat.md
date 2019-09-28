---
title: netstat
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 54bd21b7e96275d329e45e825971d9236488c793
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373260"
---
# <a name="netstat"></a>netstat

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza le connessioni TCP attive, le porte su cui il computer è in ascolto, le statistiche Ethernet, la tabella di routing IP, le statistiche IPv4 (per i protocolli IP, ICMP, TCP e UDP) e le statistiche IPv6 (per i protocolli IPv6, ICMPv6, TCP su IPv6 e UDP su IPv6). Usato senza parametri, **netstat** Visualizza le connessioni TCP attive. 

## <a name="syntax"></a>Sintassi
```
netstat [-a] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<Interval>]
```

### <a name="parameters"></a>Parametri

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
    -   stato Indica lo stato di una connessione TCP. Gli stati possibili sono i seguenti: CLOSE_WAIT CLOSED stabilita FIN_WAIT_1 FIN_WAIT_2 LAST_ACK listEN SYN_RECEIVED SYN_SEND timeD_WAIT per ulteriori informazioni sugli stati di una connessione TCP, vedere la specifica RFC 793.
-   Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.

## <a name="BKMK_Examples"></a>Esempi
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

## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
