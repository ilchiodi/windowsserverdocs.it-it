---
title: nbtstat
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4e670b1490f1c4c54b8cf377d48755849faa16f8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437126"
---
# <a name="nbtstat"></a>nbtstat

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cache dei nomi NetBIOS consente di visualizzare le statistiche di protocollo TCP/IP (NetBT), le tabelle di nome NetBIOS per il computer locale e i computer remoti e NetBIOS. **nbtstat** consente l'aggiornamento della cache il nome NetBIOS e i nomi registrati con Windows Internet Name Service (WINS). Se utilizzato senza parametri, **nbtstat** Visualizza la Guida. 

## <a name="syntax"></a>Sintassi

```
nbtstat [/a <remoteName>] [/A <IPaddress>] [/c] [/n] [/r] [/R] [/RR] [/s] [/S] [<Interval>]
```

### <a name="parameters"></a>Parametri

|    Parametro    |                                                                                                                         Descrizione                                                                                                                         |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /a <remoteName> |    Visualizza la tabella dei nomi NetBIOS di un computer remoto, in cui *remoteName* è il nome NetBIOS del computer remoto. La tabella dei nomi NetBIOS è l'elenco di nomi NetBIOS che corrisponde alle applicazioni NetBIOS in esecuzione in tale computer.     |
| /A <IPaddress>  |                                                           Visualizza la tabella dei nomi NetBIOS di un computer remoto, specificato tramite l'indirizzo IP (in notazione decimale) del computer remoto.                                                            |
|       /c        |                                                                        Consente di visualizzare il contenuto di NetBIOS nome cache, la tabella dei nomi NetBIOS e i relativi indirizzi IP risolti.                                                                         |
|       /n        |                                            Visualizza la tabella dei nomi NetBIOS del computer locale. Lo stato del **registrato** indica che il nome viene registrato da broadcast o con un server WINS.                                             |
|       /r        |      Visualizza le statistiche di risoluzione dei nome NetBIOS. In un computer che eseguono Windows XP o Windows Server 2003 che è configurato per usare WINS, questo parametro restituisce il numero di nomi che sono stati risolti e registrati tramite broadcast e WINS.       |
|       /R        |                                                                      Elimina il contenuto della cache del nome NetBIOS e quindi ricaricato il #tagged le voci dal **Lmhosts** file.                                                                      |
|       /RR       |                                                                           Rilascia e aggiorna i nomi NetBIOS per il computer locale che è stato registrato con i server WINS.                                                                            |
|       /s        |                                                                          Visualizza sessioni di client e server NetBIOS, il tentativo di convertire l'indirizzo IP di destinazione a un nome.                                                                           |
|       /S        |                                                                          Consente di visualizzare le sessioni client e server NetBIOS, elencando i computer remoti da solo indirizzo IP di destinazione.                                                                          |
|   <Interval>    | Viene visualizzata nuovamente statistiche selezionate, la sospensione il numero di secondi specificato nel *intervallo* tra ogni visualizzazione. Premere CTRL + C per arrestare la visualizzazione delle statistiche. Se questo parametro viene omesso, **nbtstat** vengono stampate le informazioni di configurazione corrente una sola volta. |
|       /?        |                                                                                                            Visualizza la guida al prompt dei comandi.                                                                                                             |

## <a name="remarks"></a>Note

-   **nbtstat** parametri della riga di comando sono tra maiuscole e minuscole.

-   La tabella seguente descrive le intestazioni di colonna che vengono generate da **nbtstat**:

    |Prua|Descrizione|
    |------|--------|
    |Input|Il numero di byte ricevuti.|
    |Output|Il numero di byte inviati.|
    |In/Out|Se la connessione è dal computer (in uscita) o da un altro computer nel computer locale (in ingresso).|
    |Ciclo di vita|Il tempo rimanente che verrà inserito una voce della cache della tabella nome prima di eliminarlo.|
    |Nome locale|Il nome NetBIOS locale associato alla connessione.|
    |Host remoto|Il nome o l'indirizzo IP associato al computer remoto.|
    |<03>|L'ultimo byte di un nome NetBIOS convertito in formato esadecimale. Ogni nome NetBIOS è 16 caratteri. L'ultimo byte spesso significato speciale in quanto lo stesso nome potrebbe essere presente più volte in un computer, che differisce solo per l'ultimo byte. Ad esempio, < 20 > è uno spazio in testo ASCII.|
    |type|Il tipo del nome. Un nome può essere un nome univoco o un nome di gruppo.|
    |Stato|Se il servizio NetBIOS del computer remoto è in esecuzione (registrato) o un nome computer duplicato ha registrato il servizio stesso (conflitto).|
    |Stato|Lo stato delle connessioni di NetBIOS.|

-   Nella tabella seguente vengono descritti i possibili stati di connessione NetBIOS:

    |Stato|Descrizione|
    |-----|--------|
    |Connesso|È stata stabilita una sessione.|
    |Associate|Un endpoint della connessione è stato creato e associato a un indirizzo IP.|
    |in attesa|Questo endpoint è disponibile per una connessione in ingresso.|
    |Idle|Questo endpoint è stato aperto, ma non può ricevere le connessioni.|
    |La connessione|Una sessione è in fase di connessione e il mapping degli indirizzi IP al nome dell'oggetto di destinazione è in fase di risoluzione.|
    |Accettazione|Una sessione in ingresso in fase di accettazione e verrà connessa a breve.|
    |La riconnessione|Una sessione viene effettuato un tentativo di riconnessione (operazione non è riuscita per la connessione al primo tentativo).|
    |In uscita|Una sessione è in fase di connessione e la connessione TCP è in corso la creazione.|
    |In entrata|Una sessione in ingresso è in fase di connessione.|
    |Disconnessione|Una sessione è in corso la disconnessione.|
    |Disconnected|Il computer locale è rilasciato una disconnessione ed è in attesa di conferma dal sistema remoto.|

-   Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.

## <a name="BKMK_Examples"></a>Esempi
Per visualizzare la tabella dei nomi NetBIOS del computer remoto con il nome NetBIOS del computer di CORP07, digitare:

```
nbtstat /a CORP07
```

Per visualizzare la tabella dei nomi NetBIOS del computer remoto assegnato l'indirizzo IP 10.0.0.99, digitare:

```
nbtstat /A 10.0.0.99
```

Per visualizzare la tabella dei nomi NetBIOS del computer locale, digitare:

```
nbtstat /n
```

Per visualizzare il contenuto del computer locale nella cache dei nomi NetBIOS, digitare:

```
nbtstat /c
```

Ripulire la cache dei nomi NetBIOS e ricaricare il # pre-tagged voci nel file Lmhosts locale, digitare:

```
nbtstat /R
```

Per rilasciare i nomi NetBIOS registrati con il server WINS e ripeterne la registrazione, digitare:

```
nbtstat /RR
```

Per visualizzare le statistiche di sessione NetBIOS in base all'indirizzo IP ogni cinque secondi, digitare:

```
nbtstat /S 5
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)


