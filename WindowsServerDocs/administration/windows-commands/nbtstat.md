---
title: nbtstat
description: Argomento di riferimento per il comando nbtstat, che visualizza le statistiche del protocollo NetBIOS su TCP/IP (NetBT), le tabelle dei nomi NetBIOS sia per il computer locale che per i computer remoti e per la cache dei nomi NetBIOS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1d2ea99e-72f1-471f-9525-d2c49bf3be82
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e205013dc5716b76981e0c9bae667d48802dfc74
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354321"
---
# <a name="nbtstat"></a>nbtstat

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza le statistiche del protocollo NetBIOS su TCP/IP (NetBT), le tabelle dei nomi NetBIOS per il computer locale e i computer remoti e la cache dei nomi NetBIOS. Questo comando consente inoltre di aggiornare la cache dei nomi NetBIOS e i nomi registrati con WINS (Windows Internet Name Service). Usato senza parametri, questo comando Visualizza le informazioni della guida.

Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.

## <a name="syntax"></a>Sintassi

```
nbtstat [/a <remotename>] [/A <IPaddress>] [/c] [/n] [/r] [/R] [/RR] [/s] [/S] [<interval>]
```

#### <a name="parameters"></a>Parametri

| Parametro | Description |
| --------- | ----------- |
| /a`<remotename>` | Visualizza la tabella dei nomi NetBIOS di un computer remoto, dove *RemoteName* è il nome del computer NetBIOS del computer remoto. La tabella dei nomi NetBIOS è l'elenco di nomi NetBIOS che corrisponde alle applicazioni NetBIOS in esecuzione nel computer. |
| /A`<IPaddress>` | Visualizza la tabella dei nomi NetBIOS di un computer remoto, specificato dall'indirizzo IP (in notazione decimale punteggiata) del computer remoto. |
| /C | Visualizza il contenuto della cache dei nomi NetBIOS, la tabella dei nomi NetBIOS e i relativi indirizzi IP risolti. |
| /n | Consente di visualizzare la tabella dei nomi NetBIOS del computer locale. Lo stato di **registrato** indica che il nome è registrato tramite broadcast o con un server WINS. |
| /r | Visualizza le statistiche di risoluzione dei nomi NetBIOS. |
| /R | Elimina il contenuto della cache dei nomi NetBIOS e quindi ricarica le voci precontrassegnate dal file **LMHOSTS** . |
| /RR | Rilascia e aggiorna i nomi NetBIOS per il computer locale registrato con i server WINS. |
| /s | Visualizza le sessioni client e server NetBIOS, tentando di convertire l'indirizzo IP di destinazione in un nome. |
| /S | Visualizza le sessioni client e server NetBIOS, elencando solo i computer remoti in base all'indirizzo IP di destinazione. |
| `<interval>` | Visualizza le statistiche selezionate, sospendendo il numero di secondi specificato nell' *intervallo* tra ogni visualizzazione. Premere CTRL + C per arrestare la visualizzazione delle statistiche. Se questo parametro viene omesso, **nbtstat** stampa le informazioni di configurazione correnti una sola volta. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Commenti

- Per i parametri della riga di comando **nbtstat** viene fatta distinzione tra maiuscole e minuscole.

- Le intestazioni di colonna generate dal comando **nbtstat** includono:

    | Prua | Descrizione |
    | ------- | ----------- |
    | Input | Numero di byte ricevuti. |
    | Output | Numero di byte inviati. |
    | Ingresso/Uscita | Indica se la connessione viene sttratta dal computer (in uscita) o da un altro computer al computer locale (in ingresso). |
    | Life | Tempo rimanente per il quale una voce della cache della tabella dei nomi vivrà prima di essere eliminata. |
    | Nome locale | Nome NetBIOS locale associato alla connessione. |
    | Host remoto | Nome o indirizzo IP associato al computer remoto. |
    | `<03>` | Ultimo byte di un nome NetBIOS convertito in esadecimale. Ogni nome NetBIOS è lungo 16 caratteri. Questo ultimo byte ha spesso un significato speciale perché lo stesso nome può essere presente più volte in un computer, che differisce solo per l'ultimo byte. Ad esempio, `<20>` è uno spazio nel testo ASCII. |
    | tipo | Tipo di nome. Un nome può essere un nome univoco o un nome di gruppo. |
    | Stato | Se il servizio NetBIOS nel computer remoto è in esecuzione (registrato) o se un nome di computer duplicato ha registrato lo stesso servizio (conflitto). |
    | State | Stato delle connessioni NetBIOS. |

- I possibili stati di connessione NetBIOS includono:

    | State | Descrizione |
    | ------- | ----------- |
    | Connesso | È stata stabilita una sessione. |
    | in ascolto | Questo endpoint è disponibile per una connessione in ingresso. |
    | Idle | Questo endpoint è stato aperto ma non può ricevere connessioni. |
    | Connecting | Una sessione si trova nella fase di connessione e il mapping tra nome e indirizzo IP della destinazione è in fase di risoluzione. |
    | Accettazione | Una sessione in ingresso è attualmente in fase di accettazione e verrà connessa a breve. |
    | Riconnessione | Una sessione sta provando a riconnettersi (non è stato possibile connettersi al primo tentativo). |
    | In uscita | Una sessione si trova nella fase di connessione ed è in corso la creazione della connessione TCP. |
    | In ingresso | Una sessione in ingresso si trova nella fase di connessione. |
    | Disconnessione in corso | È in corso la disconnessione di una sessione. |
    | Disconnesso | Il computer locale ha emesso una disconnessione ed è in attesa di conferma dal sistema remoto. |

### <a name="examples"></a>Esempio

Per visualizzare la tabella dei nomi NetBIOS del computer remoto con il nome NetBIOS del computer *CORP07*, digitare:

```
nbtstat /a CORP07
```

Per visualizzare la tabella dei nomi NetBIOS del computer remoto assegnato all'indirizzo IP di *10.0.0.99*, digitare:

```
nbtstat /A 10.0.0.99
```

Per visualizzare la tabella dei nomi NetBIOS del computer locale, digitare:

```
nbtstat /n
```

Per visualizzare il contenuto della cache dei nomi NetBIOS del computer locale, digitare:

```
nbtstat /c
```

Per ripulire la cache dei nomi NetBIOS e ricaricare le voci precedentemente contrassegnate nel file *LMHOSTS* locale, digitare:

```
nbtstat /R
```

Per rilasciare i nomi NetBIOS registrati con il server WINS e registrarli nuovamente, digitare:

```
nbtstat /RR
```

Per visualizzare le statistiche della sessione NetBIOS in base all'indirizzo IP ogni cinque secondi, digitare:

```
nbtstat /S 5
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
