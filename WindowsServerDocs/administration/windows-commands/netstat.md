---
title: netstat
description: Argomento di riferimento per il comando netstat, che visualizza le connessioni TCP attive, le porte su cui il computer è in ascolto, le statistiche Ethernet, la tabella di routing IP, le statistiche IPv4 e le statistiche IPv6.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60e2718f-93cc-4ceb-bf0e-58a6a6e4fc8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6eae779216724d82ef7ca05026bcfd9725e6ea35
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721544"
---
# <a name="netstat"></a>netstat

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza le connessioni TCP attive, le porte su cui il computer è in ascolto, le statistiche Ethernet, la tabella di routing IP, le statistiche IPv4 (per i protocolli IP, ICMP, TCP e UDP) e le statistiche IPv6 (per i protocolli IPv6, ICMPv6, TCP su IPv6 e UDP su IPv6). Usato senza parametri, questo comando Visualizza le connessioni TCP attive.

> [!IMPORTANT]
> Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.

## <a name="syntax"></a>Sintassi

```
netstat [-a] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<interval>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -a | Consente di visualizzare tutte le connessioni TCP attive e le porte TCP e UDP su cui è in ascolto il computer. |
| -E | Visualizza le statistiche Ethernet, ad esempio il numero di byte e i pacchetti inviati e ricevuti. Questo parametro può essere combinato con **-s**. |
| -n | Visualizza le connessioni TCP attive, tuttavia, gli indirizzi e i numeri di porta vengono espressi numericamente e non viene effettuato alcun tentativo di determinare i nomi. |
| -o | Visualizza le connessioni TCP attive e include l'ID processo (PID) per ogni connessione. È possibile trovare l'applicazione in base al PID nella scheda processi di gestione attività di Windows. Questo parametro può essere combinato con **-a**, **-n**e **-p**. |
| -p`<Protocol>` | Mostra le connessioni per il protocollo specificato dal *protocollo*. In questo caso, il *protocollo* può essere TCP, UDP, TCPv6 o UDPv6. Se questo parametro viene utilizzato con **-s** per visualizzare le statistiche per protocollo, il *protocollo* può essere TCP, UDP, ICMP, IP, TCPv6, UDPv6, ICMPv6 o IPv6. |
| -S | Visualizza le statistiche per protocollo. Per impostazione predefinita, vengono visualizzate le statistiche per i protocolli TCP, UDP, ICMP e IP. Se è installato il protocollo IPv6, vengono visualizzate le statistiche per i protocolli TCP su IPv6, UDP su IPv6, ICMPv6 e IPv6. Il parametro **-p** può essere utilizzato per specificare un set di protocolli. |
| -r | Consente di visualizzare il contenuto della tabella di routing IP. Equivale al comando route print. |
| `<interval>` | Visualizza nuovamente le informazioni selezionate ogni *intervallo* di secondi. Premere CTRL + C per arrestare la rivisualizzazione. Se questo parametro viene omesso, questo comando stampa le informazioni selezionate una sola volta. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Commenti

- Il comando **netstat** fornisce le statistiche per gli elementi seguenti:

    | Parametro | Descrizione |
    | --------- | ----------- |
    | Proto | Nome del protocollo (TCP o UDP). |
    | Indirizzo locale | Indirizzo IP del computer locale e numero di porta utilizzato. Il nome del computer locale che corrisponde all'indirizzo IP e il nome della porta viene visualizzato a meno che non sia specificato il parametro **-n** . Se la porta non è ancora stata stabilita, il numero di porta viene visualizzato come asterisco (*). |
    | Indirizzo esterno | Indirizzo IP e numero di porta del computer remoto a cui è connesso il socket. I nomi che corrispondono all'indirizzo IP e alla porta vengono visualizzati a meno che non sia specificato il parametro **-n** . Se la porta non è ancora stata stabilita, il numero di porta viene visualizzato come asterisco (*). |
    | State | Indica lo stato di una connessione TCP, tra cui:<ul><li>CLOSE_WAIT</li><li>CLOSED</li><li>STABILITO</li><li>FIN_WAIT_1</li><li>FIN_WAIT_2</li><li>LAST_ACK</li><li>ASCOLTARE</li><li>SYN_RECEIVED</li><li>SYN_SEND</li><li>TIMED_WAIT</li></ul> |

### <a name="examples"></a>Esempio

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

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
