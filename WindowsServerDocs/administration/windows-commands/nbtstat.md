---
title: nbtstat
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d2ea99e-72f1-471f-9525-d2c49bf3be82
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd9148c922ac97e83b3b21bcb8b6585775505bf2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373324"
---
# <a name="nbtstat"></a>nbtstat

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza le statistiche del protocollo NetBIOS su TCP/IP (NetBT), le tabelle dei nomi NetBIOS per il computer locale e i computer remoti e la cache dei nomi NetBIOS. **nbtstat** consente l'aggiornamento della cache dei nomi NetBIOS e dei nomi registrati con WINS (Windows Internet Name Service). Usato senza parametri, **nbtstat** Visualizza la guida. 

## <a name="syntax"></a>Sintassi

```
nbtstat [/a <remoteName>] [/A <IPaddress>] [/c] [/n] [/r] [/R] [/RR] [/s] [/S] [<Interval>]
```

### <a name="parameters"></a>Parametri

|    Parametro    |                                                                                                                         Descrizione                                                                                                                         |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /a <remoteName> |    Visualizza la tabella dei nomi NetBIOS di un computer remoto, dove *RemoteName* è il nome del computer NetBIOS del computer remoto. La tabella dei nomi NetBIOS è l'elenco di nomi NetBIOS che corrisponde alle applicazioni NetBIOS in esecuzione nel computer.     |
| /A <IPaddress>  |                                                           Visualizza la tabella dei nomi NetBIOS di un computer remoto, specificato dall'indirizzo IP (in notazione decimale punteggiata) del computer remoto.                                                            |
|       /c        |                                                                        Visualizza il contenuto della cache dei nomi NetBIOS, la tabella dei nomi NetBIOS e i relativi indirizzi IP risolti.                                                                         |
|       /n        |                                            Consente di visualizzare la tabella dei nomi NetBIOS del computer locale. Lo stato di **registrato** indica che il nome è registrato tramite broadcast o con un server WINS.                                             |
|       /r        |      Visualizza le statistiche di risoluzione dei nomi NetBIOS. In un computer che esegue Windows XP o Windows Server 2003 configurato per l'utilizzo di WINS, questo parametro restituisce il numero di nomi che sono stati risolti e registrati utilizzando broadcast e WINS.       |
|       /R        |                                                                      Elimina il contenuto della cache dei nomi NetBIOS, quindi ricarica le voci con tag #PRE dal file **LMHOSTS** .                                                                      |
|       /RR       |                                                                           Rilascia e aggiorna i nomi NetBIOS per il computer locale registrato con i server WINS.                                                                            |
|       /s        |                                                                          Visualizza le sessioni client e server NetBIOS, tentando di convertire l'indirizzo IP di destinazione in un nome.                                                                           |
|       /S        |                                                                          Visualizza le sessioni client e server NetBIOS, elencando solo i computer remoti in base all'indirizzo IP di destinazione.                                                                          |
|   <Interval>    | Visualizza nuovamente le statistiche selezionate, sospendendo il numero di secondi specificato nell' *intervallo* tra ogni visualizzazione. Premere CTRL + C per arrestare la visualizzazione delle statistiche. Se questo parametro viene omesso, **nbtstat** stampa le informazioni di configurazione correnti una sola volta. |
|       /?        |                                                                                                            Visualizza la guida al prompt dei comandi.                                                                                                             |

## <a name="remarks"></a>Note

-   i parametri della riga di comando **nbtstat** fanno distinzione tra maiuscole e minuscole.

-   Nella tabella seguente vengono descritte le intestazioni di colonna generate da **nbtstat**:

    |Prua|Descrizione|
    |------|--------|
    |Input|Numero di byte ricevuti.|
    |Output|Numero di byte inviati.|
    |In/Out|Indica se la connessione viene sttratta dal computer (in uscita) o da un altro computer al computer locale (in ingresso).|
    |Vita|Tempo rimanente per il quale una voce della cache della tabella dei nomi vivrà prima di essere eliminata.|
    |Nome locale|Nome NetBIOS locale associato alla connessione.|
    |Host remoto|Nome o indirizzo IP associato al computer remoto.|
    |< 03 >|Ultimo byte di un nome NetBIOS convertito in esadecimale. Ogni nome NetBIOS è lungo 16 caratteri. Questo ultimo byte ha spesso un significato speciale perché lo stesso nome può essere presente più volte in un computer, che differisce solo per l'ultimo byte. Ad esempio, < 20 > è uno spazio nel testo ASCII.|
    |type|Tipo di nome. Un nome può essere un nome univoco o un nome di gruppo.|
    |Stato|Se il servizio NetBIOS nel computer remoto è in esecuzione (registrato) o se un nome di computer duplicato ha registrato lo stesso servizio (conflitto).|
    |State|Stato delle connessioni NetBIOS.|

-   Nella tabella seguente vengono descritti i possibili stati di connessione NetBIOS:

    |State|Descrizione|
    |-----|--------|
    |Connesso|È stata stabilita una sessione.|
    |associato|Un endpoint di connessione è stato creato e associato a un indirizzo IP.|
    |in ascolto|Questo endpoint è disponibile per una connessione in ingresso.|
    |Idle|Questo endpoint è stato aperto ma non può ricevere connessioni.|
    |Connessione|Una sessione si trova nella fase di connessione e il mapping tra nome e indirizzo IP della destinazione è in fase di risoluzione.|
    |Accettazione|Una sessione in ingresso è attualmente in fase di accettazione e verrà connessa a breve.|
    |Riconnessione|Una sessione sta provando a riconnettersi (non è stato possibile connettersi al primo tentativo).|
    |In uscita|Una sessione si trova nella fase di connessione ed è in corso la creazione della connessione TCP.|
    |In ingresso|Una sessione in ingresso si trova nella fase di connessione.|
    |Scollegare|È in corso la disconnessione di una sessione.|
    |Disconnected|Il computer locale ha emesso una disconnessione ed è in attesa di conferma dal sistema remoto.|

-   Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.

## <a name="BKMK_Examples"></a>Esempi
Per visualizzare la tabella dei nomi NetBIOS del computer remoto con il nome NetBIOS del computer CORP07, digitare:

```
nbtstat /a CORP07
```

Per visualizzare la tabella dei nomi NetBIOS del computer remoto assegnato all'indirizzo IP di 10.0.0.99, digitare:

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

Per ripulire la cache dei nomi NetBIOS e ricaricare le voci con tag #PRE nel file LMHOSTS locale, digitare:

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

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)


