---
title: at
description: Argomenti di Windows per i comandi e i programmi di **in** -schedules da eseguire in un computer a una data e ora specificate.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6c8a7497b1276cf46177fa72824afb8ed238776a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382617"
---
# <a name="at"></a>at

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pianifica i comandi e i programmi da eseguire in un computer a una data e ora specificate. È **possibile utilizzare solo quando è in esecuzione** il servizio Schedule. Usato senza parametri, **in** elenca i comandi pianificati.
## <a name="syntax"></a>Sintassi
```
at [\\computername] [[id] [/delete] | /delete [/yes]]
at [\\computername] <time> [/interactive] [/every:date[,...] | /next:date[,...]] <command>
```
## <a name="parameters"></a>Parametri

|      Parametro       |                                                                                                                                                                                                               Descrizione                                                                                                                                                                                                                |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \\\\\<nomecomputer\> |                                                                                                                                                        Specifica un computer remoto. Se si omette questo parametro, **in** Pianifica i comandi e i programmi nel computer locale.                                                                                                                                                        |
|        ID \<\>        |                                                                                                                                                                                   Specifica il numero di identificazione assegnato a un comando pianificato.                                                                                                                                                                                   |
|       /delete        |                                                                                                                                                                Annulla un comando pianificato. Se si omette l' *ID*, tutti i comandi pianificati nel computer vengono annullati.                                                                                                                                                                |
|         /Yes         |                                                                                                                                                                               Risponde Sì a tutte le query del sistema quando si eliminano gli eventi pianificati.                                                                                                                                                                               |
|       \<time\>       |                                                                                                                                          Specifica l'ora in cui si desidera eseguire il comando. il tempo è espresso in ore: minuti nella notazione di 24 ore (ovvero 00:00 [mezzanotte] a 23:59).                                                                                                                                          |
|     /Interactive     |                                                                                                                                                                  Consente al *comando* di interagire con il desktop dell'utente che ha eseguito l'accesso al momento dell'esecuzione del *comando* .                                                                                                                                                                  |
|       ogni        |                                                                                                                                                    Esegue il *comando* ogni giorno o giorno della settimana o del mese specificato (ad esempio, ogni giovedì o il terzo giorno di ogni mese).                                                                                                                                                    |
|       \<date\>       |                                                  Specifica la data in cui si desidera eseguire il comando. È possibile specificare uno o più giorni della settimana (ovvero, digitare **M**,**T**,**W**,**th**,**F**,**S**,**su**) o uno o più giorni del mese (ovvero, digitare da 1 a 31). Separare più voci di data con virgole. Se si omette *Data*, **in** viene utilizzato il giorno corrente del mese.                                                  |
|        prossimo        |                                                                                                                                                                              Esegue il *comando* alla successiva occorrenza del giorno (ad esempio, giovedì successivo).                                                                                                                                                                              |
|     comando \<\>      | Specifica il comando di Windows, il programma (ovvero il file con estensione exe o com) o il programma batch (ovvero il file con estensione bat o cmd) che si desidera eseguire. Quando il comando richiede un percorso come argomento, usare il percorso assoluto, ovvero l'intero percorso che inizia con la lettera di unità. Se il comando si trova in un computer remoto, specificare Universal Naming Convention notazione (UNC) per il nome del server e della condivisione, anziché una lettera di unità remota. |
|          /?          |                                                                                                                                                                                                   Visualizza la guida al prompt dei comandi.                                                                                                                                                                                                   |

## <a name="remarks"></a>Osservazioni
- **Schtasks** è un altro strumento di pianificazione da riga di comando che è possibile utilizzare per creare e gestire attività pianificate. Per ulteriori informazioni su **Schtasks**, vedere Argomenti correlati.
- Utilizzo **di alle**  
  Per utilizzare **in**, è necessario essere un membro del gruppo Administrators locale.
- Caricamento di cmd. exe  
  **in** non carica automaticamente cmd. exe, l'interprete dei comandi, prima di eseguire i comandi. Se non si esegue un file eseguibile (exe), è necessario caricare in modo esplicito cmd. exe all'inizio del comando come indicato di seguito: **cmd/c dir > c:\test.out**
- Visualizzazione di comandi pianificati  
  Quando si utilizza **at** senza opzioni della riga di comando, le attività pianificate vengono visualizzate in una tabella con un formato simile al seguente:
  ```
  Status  ID   Day        time        Command Line
  OK      1    Each F     4:30 PM     net send group leads status due
  OK      2    Each M     12:00 AM    chkstor > check.file
  OK      3    Each F     11:59 PM    backup2.bat
  ```
- Inclusione del numero di identificazione (*ID*)  
  Quando si include il numero di identificazione (*ID*) con **at** al prompt dei comandi, le informazioni per una singola voce vengono visualizzate in un formato simile al seguente:  
  ```
  Task ID:      1
  Status:       OK
  Schedule:     Each  F
  time of Day:  4:30 PM
  Command:      net send group leads status due
  ```
  Dopo aver pianificato un comando con **at, in**particolare un comando con opzioni della riga di comando, verificare che la sintassi del comando sia corretta digitando senza opzioni della riga di comando. Se le informazioni nella colonna della riga di comando non sono corrette, eliminare il comando e digitarlo di tipo. Se è ancora errato, digitare nuovamente il comando con un minor numero di opzioni della riga di comando.
- Visualizzazione dei risultati  
  Comandi pianificati con **nei processi in** background Esegui come. L'output non viene visualizzato nella schermata del computer. Per reindirizzare l'output a un file, utilizzare il simbolo di reindirizzamento (>). Se si reindirizza l'output a un file, è necessario usare il simbolo di escape (^) prima del simbolo di reindirizzamento, indipendentemente dal fatto che si usi **at** dalla riga di comando o in un file batch. Per reindirizzare l'output a output. text, ad esempio, digitare:  
  `at 14:45 c:\test.bat ^>c:\output.txt`  
  La directory corrente per il comando in esecuzione è la cartella SystemRoot.
- Modifica dell'ora di sistema  
  Se si modifica l'ora di sistema in un computer dopo aver pianificato l'esecuzione di un comando con **in**, sincronizzare il **in** utilità di pianificazione con l'ora di sistema modificata digitando senza opzioni della **riga di comando** .
- Archiviazione dei comandi  
  I comandi pianificati vengono archiviati nel registro di sistema. Di conseguenza, le attività pianificate non vengono perse se si riavvia il servizio Schedule.
- Connessione alle unità di rete  
  Non usare un'unità reindirizzata per i processi pianificati che accedono alla rete. Il servizio di pianificazione potrebbe non essere in grado di accedere all'unità reindirizzata oppure l'unità reindirizzata potrebbe non essere presente se un utente diverso è connesso al momento dell'esecuzione dell'attività pianificata. Usare invece i percorsi UNC per i processi pianificati. Ad esempio:  
  `at 1:00pm my_backup \\\server\share`  
  Non utilizzare la sintassi seguente, dove **x:** è una connessione eseguita dall'utente:  
  `at 1:00pm my_backup x:`  
  Se si pianifica un comando **at** che utilizza una lettera di unità per connettersi a una directory condivisa, includere un comando **at** per disconnettere l'unità al termine dell'utilizzo dell'unità. Se l'unità non è disconnessa, la lettera di unità assegnata non è disponibile al prompt dei comandi.
- Attività arrestate dopo 72 ore  
  Per impostazione predefinita, le attività pianificate tramite il comando **at** si interrompono dopo 72 ore. È possibile modificare il registro di sistema per modificare questo valore predefinito.
  1.  avviare l'editor del registro di sistema (Regedit. exe).
  2.  Individuare e fare clic sulla chiave seguente nel registro di sistema: **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\schedule**
  3.  Scegliere Aggiungi valore dal menu modifica, quindi aggiungere il valore del registro di sistema seguente: nome valore: tipo di dati atTaskMaxHours: reg_DWOrd radice: dati del valore decimale: 0. Il valore 0 nel campo dati valore indica nessun limite, non viene arrestato. I valori compresi tra 1 e 99 indicano il numero di ore.
  **Attenzione**
- La modifica non corretta del Registro di sistema potrebbe danneggiare gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.
- Utilità di pianificazione e il comando **at**  
  È possibile utilizzare la cartella attività pianificate per visualizzare o modificare le impostazioni di un'attività creata tramite il comando **at** . Quando si pianifica un'attività usando il comando **at** , l'attività viene elencata nella cartella attività pianificate, con un nome simile al seguente:**At3478**. Tuttavia, se si modifica un'attività nella cartella attività pianificate, questa viene aggiornata a una normale attività pianificata. L'attività non è più visibile al comando **at** e l'impostazione dell'account at non è più applicabile. È necessario immettere in modo esplicito un account utente e una password per l'attività.
  ## <a name="examples"></a>Esempi
  Per visualizzare un elenco di comandi pianificati nel server marketing, digitare:

`at \\marketing`

Per ulteriori informazioni su un comando con il numero di identificazione 3 nel server Corp, digitare:

`at \\corp 3`

Per pianificare un comando net share da eseguire sul server Corp alle 8:00 AM reindirizzare l'elenco al server di manutenzione, nella directory condivisa report e nel file Corp. txt, digitare:

`at \\corp 08:00 cmd /c "net share reports=d:\marketing\reports >> \\maintenance\reports\corp.txt"`

Per eseguire il backup del disco rigido del server Marketing su un'unità nastro a mezzanotte ogni cinque giorni, creare un programma batch denominato Archive. cmd, che contiene i comandi di backup, quindi pianificare l'esecuzione del programma batch, digitare:

`at \\marketing 00:00 /every:5,10,15,20,25,30 archive`

Per annullare tutti i comandi pianificati nel server corrente, deselezionare le informazioni relative **alla** pianificazione nel modo seguente:

`at /delete`

Per eseguire un comando che non è un file eseguibile (ad esempio, con estensione exe), anteporre cmd. exe al comando per caricare cmd. exe come **indicato di seguito** :

`cmd /c dir > c:\test.out`
