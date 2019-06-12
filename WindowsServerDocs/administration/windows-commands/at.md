---
title: at
description: "Argomento i comandi di Windows per **a** : Pianifica comandi e programmi da eseguire su un computer in una data e l'intervallo di tempo specificato."
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ff18fd16-9437-4c53-8794-bfc67f5256b3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fc9d9f3d008db1bb85bfb6afa0308834c929b5f0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435284"
---
# <a name="at"></a>at

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pianifica comandi e programmi per l'esecuzione in un computer in una data e l'intervallo di tempo specificato. È possibile usare **a** solo quando il servizio di pianificazione è in esecuzione. Se utilizzato senza parametri, **a** Elenca i comandi pianificati.
## <a name="syntax"></a>Sintassi
```
at [\\computername] [[id] [/delete] | /delete [/yes]]
at [\\computername] <time> [/interactive] [/every:date[,...] | /next:date[,...]] <command>
```
## <a name="parameters"></a>Parametri

|      Parametro       |                                                                                                                                                                                                               Descrizione                                                                                                                                                                                                                |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \\\\\<computerName\> |                                                                                                                                                        Specifica un computer remoto. Se si omette questo parametro, **a** pianifica comandi e programmi nel computer locale.                                                                                                                                                        |
|        \<id\>        |                                                                                                                                                                                   Specifica il numero di identificazione assegnato a un comando pianificato.                                                                                                                                                                                   |
|       /delete        |                                                                                                                                                                Annulla un comando pianificato. Se si omette *ID*, tutti i comandi nel computer pianificati vengono annullati.                                                                                                                                                                |
|         /yes         |                                                                                                                                                                               Risposte Sì a tutte le query dal sistema quando si elimina gli eventi pianificati.                                                                                                                                                                               |
|       \<time\>       |                                                                                                                                          Specifica il tempo quando si desidera eseguire il comando. ora è espresso in ore: minuti nella notazione 24 ore (vale a dire 00:00 (mezzanotte) a 23:59).                                                                                                                                          |
|     / interattivo     |                                                                                                                                                                  Consente *comandi* per interagire con il desktop dell'utente che ha effettuato l'accesso al momento *comando* viene eseguito.                                                                                                                                                                  |
|       / ogni:        |                                                                                                                                                    Esecuzioni *comando* in ogni giorno specificato o i giorni della settimana o mese (ad esempio, ogni giovedì, o il terzo giorno del mese).                                                                                                                                                    |
|       \<date\>       |                                                  Specifica la data desiderata per eseguire il comando. È possibile specificare uno o più giorni della settimana (ovvero, type **M**,**T**,**W**,**Th**,**F**,**S**,**unità di streaming**) o uno o più giorni del mese (ovvero, type 1 a 31). Separare più voci con una virgola. Se si omette *data*, **a** utilizza il giorno del mese corrente.                                                  |
|        /next:        |                                                                                                                                                                              Esecuzioni *comando* alla successiva occorrenza del giorno (ad esempio, il giovedì successivo).                                                                                                                                                                              |
|     \<command\>      | Specifica il comando di Windows, programma (vale a dire, i file. com o .exe) o file batch (vale a dire, file con estensione bat o cmd) che si desidera eseguire. Quando il comando richiede un percorso come argomento, usare il percorso assoluto (vale a dire, l'intero percorso che inizia con la lettera di unità). Se il comando in un computer remoto, specificare la notazione Universal Naming Convention (UNC) per il server e della condivisione di nome, anziché una lettera di unità remota. |
|          /?          |                                                                                                                                                                                                   Visualizza la guida al prompt dei comandi.                                                                                                                                                                                                   |

## <a name="remarks"></a>Note
- **Schtasks** è un altro strumento di pianificazione della riga di comando che è possibile usare per creare e gestire le attività pianificate. Per altre informazioni sulle **schtasks**, vedere Argomenti correlati.
- Usando **in**  
  Per utilizzare **a**, è necessario essere un membro del gruppo Administrators locale.
- Il caricamento Cmd.exe  
  **in** non carica automaticamente Cmd.exe, l'interprete dei comandi, prima di eseguire comandi. Se non si usa un file eseguibile (.exe), è necessario caricare esplicitamente Cmd.exe all'inizio del comando come indicato di seguito: **cmd /c dir > c:\test.out**
- Comandi di visualizzazione pianificata  
  Quando si usa **a** senza opzioni della riga di comando, le attività pianificate vengono visualizzati in una tabella con un formato simile al seguente:
  ```
  Status  ID   Day        time        Command Line
  OK      1    Each F     4:30 PM     net send group leads status due
  OK      2    Each M     12:00 AM    chkstor > check.file
  OK      3    Each F     11:59 PM    backup2.bat
  ```
- Incluso il numero di identificazione (*ID*)  
  Quando si include il numero di identificazione (*ID*) con **a** un prompt dei comandi, le informazioni per una singola voce viene visualizzata in un formato simile al seguente:  
  ```
  Task ID:      1
  Status:       OK
  Schedule:     Each  F
  time of Day:  4:30 PM
  Command:      net send group leads status due
  ```
  Dopo che si pianifica un comando con **alla**, in particolare un comando che include le opzioni della riga di comando, verificare che la sintassi del comando sia corretta digitando **alla** senza opzioni della riga di comando. Se le informazioni nella colonna della riga di comando non sono corretta, digitare di nuovo il comando delete. Se non è ancora corretto, digitare nuovamente il comando con un minor numero di opzioni della riga di comando.
- Visualizzazione dei risultati  
  Comandi pianificati in base alla **a** eseguiti come processi in background. Output non viene visualizzato sullo schermo del computer. Per reindirizzare l'output in un file, usare il simbolo di reindirizzamento (>). Se si reindirizza l'output in un file, è necessario usare il simbolo di escape (^) prima del simbolo di reindirizzamento, se si usa **a** nella riga di comando o in un file batch. Ad esempio, per reindirizzare l'output in risultati. txt, digitare:  
  `at 14:45 c:\test.bat ^>c:\output.txt`  
  La directory corrente per il comando in esecuzione è la cartella systemroot.
- Modifica ora di sistema  
  Se si modifica l'ora di sistema in un computer dopo la pianificazione di un comando da eseguire con **alla**, sincronizzare il **alla** utilità di pianificazione con l'ora di sistema modificato digitando **in** senza opzioni della riga di comando.
- L'archiviazione dei comandi  
  I comandi pianificati vengono archiviati nel Registro di sistema. Di conseguenza, non perdere le attività pianificate se si riavvia il servizio di pianificazione.
- La connessione alla rete unità  
  Non utilizzare un'unità reindirizzata per i processi pianificati che accedono alla rete. Il servizio di pianificazione potrebbe non essere in grado di accedere all'unità reindirizzata o unità reindirizzata potrebbe non essere presente se un altro utente è connesso al momento del che esecuzione dell'attività pianificata. Usare invece i percorsi UNC per i processi pianificati. Ad esempio:  
  `at 1:00pm my_backup \\\server\share`  
  Non utilizzare la sintassi seguente, dove **x:.** è una connessione effettuata dall'utente:  
  `at 1:00pm my_backup x:`  
  Se si pianifica un' **alla** comando che utilizza una lettera di unità per connettersi a una directory condivisa, includere un' **a** comando per disconnettere l'unità al termine dell'uso dell'unità. Se l'unità non viene disconnesso, la lettera di unità assegnata non è disponibile al prompt dei comandi.
- Interruzione delle operazioni dopo 72 ore  
  Per impostazione predefinita, le attività pianificate usando il **a** comando stop dopo 72 ore. È possibile modificare il Registro di sistema per modificare questo valore predefinito.
  1.  avviare l'editor del Registro di sistema (regedit.exe).
  2.  Individuare e selezionare la seguente chiave del Registro di sistema: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Schedule**
  3.  Nel menu Modifica, fare clic su Aggiungi valore e quindi aggiungere il valore del Registro di sistema seguente: Nome valore: atTaskMaxHours tipo di dati: reg_DWOrd radice: Dati del valore decimale: 0. Un valore pari a 0 nel campo dati valore indica nessun limite, non viene arrestato. Valori compresi tra 1 e 99 indica il numero di ore.
  **Attenzione**
- La modifica non corretta del Registro di sistema potrebbe danneggiare gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.
- Utilità di pianificazione e la **a** comando  
  È possibile usare la cartella attività pianificate per visualizzare o modificare le impostazioni di un'attività che è stato creato utilizzando il **a** comando. Quando si pianifica un'attività utilizzando la **alla** comando, l'attività è elencato nella cartella delle attività pianificate, con un nome simile al seguente:**at3478**. Tuttavia, se si modifica all'attività tramite la cartella attività pianificate, viene aggiornato a una normale operazione pianificata. L'attività non è più visibile per il **a** comando e l'account impostazione non è più applicabile a esso. È necessario immettere in modo esplicito un account utente e una password per l'attività.
  ## <a name="examples"></a>Esempi
  Per visualizzare un elenco di comandi pianificati per il server di Marketing, digitare:

`at \\marketing`

Per altre informazioni su un comando con il numero di identificazione 3 nel server di Corp, digitare:

`at \\corp 3`

Per pianificare un comando net share per l'esecuzione nel server di Corp in 8.00 e reindirizzare l'elenco verso il server di manutenzione, nella directory condivisa i report e il file org. txt, tipo:

`at \\corp 08:00 cmd /c "net share reports=d:\marketing\reports >> \\maintenance\reports\corp.txt"`

Per eseguire il backup del disco rigido del server di Marketing in un'unità nastro a mezzanotte ogni cinque giorni, creare un file batch chiamato cmd contenente i comandi di backup, e quindi pianificare l'esecuzione del programma di batch, digitare:

`at \\marketing 00:00 /every:5,10,15,20,25,30 archive`

Per annullare tutti i comandi pianificati nel server corrente, cancellare il **a** le informazioni di pianificazione come indicato di seguito:

`at /delete`

Per eseguire un comando che non è un file eseguibile (vale a dire .exe), anteporre il comando con **cmd /c** caricare Cmd.exe come indicato di seguito:

`cmd /c dir > c:\test.out`
