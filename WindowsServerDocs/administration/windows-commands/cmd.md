---
title: cmd
description: Argomento di riferimento per il comando cmd, che avvia una nuova istanza dell'interprete dei comandi, cmd. exe.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ec588db-31a9-4a73-a970-65a2c6f4abbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8d381dd56d6648f749cd4a19d71422897e4b9b05
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712582"
---
# <a name="cmd"></a>cmd

Avvia una nuova istanza dell'interprete dei comandi, Cmd.exe. Se utilizzata senza parametri, **cmd** vengono visualizzate le informazioni sul copyright e versione del sistema operativo.

## <a name="syntax"></a>Sintassi

```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<b><f> | <f>}] [/e:{on | off}] [/f:{on | off}] [/v:{on | off}] [<string>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /C | Esegue il comando specificato dalla *stringa* e quindi si arresta. |
| /k | Esegue il comando specificato dalla *stringa* e continua. |
| /s | Modifica il trattamento della *stringa* dopo **/c** o **/k**. |
| /q | Disattiva l'eco. |
| /d | Disattiva l'esecuzione dei comandi di esecuzione automatica. |
| /a | Formatta l'output del comando interno a una pipe o un file come American National Standards Institute (ANSI). |
| /U | Formatta l'output del comando interno a una pipe o un file come Unicode. |
| /t: {`<b><f>` | `<f>`} | Imposta i colori di sfondo (*b*) e di primo piano (*f*). |
| /e: in | Attiva le estensioni del comando. |
| /e: off | Disabilita le estensioni ai comandi. |
| /f: in | Consente il completamento del nome file e directory. |
| /f: off | Disabilita il completamento del nome file e directory. |
| /v: in | Consente di ritardare l'espansione della variabile di ambiente. |
| /v: off | Disabilita ritardata espansione della variabile di ambiente. |
| `<string>` | Specifica il comando che si desidera eseguire. |
| /? | Visualizza la guida al prompt dei comandi. |

Nella tabella seguente sono elencate le cifre esadecimali valide che è possibile utilizzare come valori per `<b>` e `<f>`:

| valore | Colore |
| ----- | ----- |
| 0 | Nero |
| 1 | Blu |
| 2 | Green |
| 3 | Verde acqua |
| 4 | Rosso |
| 5 | Viola |
| 6 | Giallo |
| 7 | bianco |
| 8 | Grigio |
| 9 | Azzurro |
| a | Verde chiaro |
| b | Azzurro |
| c | Rosso |
| d | Viola chiaro |
| e | Giallo |
| f | Sfondo bianco |

## <a name="remarks"></a>Osservazioni

- Per usare più comandi per `<string>`, separarli dal separatore **&&** di comandi e racchiuderli tra virgolette. Ad esempio:

    ```
    "<command1>&&<command2>&&<command3>"
    ```

- Se si specifica **/c** o **/k**, i processi **cmd** , il resto della *stringa*e le virgolette vengono conservati solo se vengono soddisfatte tutte le condizioni seguenti:

    - Non si usa inoltre **/s**.

    - Utilizzare un unico set di virgolette.

    - Non usare caratteri speciali all'interno delle virgolette (ad esempio:  & < >  () @ ^ |).

    - Utilizzare uno o più caratteri spazio vuoto tra virgolette.

    - La *stringa* racchiusa tra virgolette è il nome di un file eseguibile.

    Se le condizioni precedenti non vengono soddisfatte, la *stringa* viene elaborata esaminando il primo carattere per verificare se si tratta di una virgoletta di apertura. Se il primo carattere è un segno di virgolette di apertura, si è rimosso insieme virgolette di chiusura. Qualsiasi testo che segue le virgolette di chiusura viene mantenuto.

- Se non si specifica **/d** nella *stringa*, cmd. exe cercherà le sottochiavi del registro di sistema seguenti:

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\AutoRun\ REG_SZ**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\AutoRun\ REG_EXPAND_SZ**

    Se sono presenti una o entrambe le sottochiavi del registro di sistema, vengono eseguite prima di tutte le altre variabili.

    > [!CAUTION]
    > La modifica non corretta del Registro di sistema potrebbe danneggiare gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.

- È possibile disabilitare le estensioni dei comandi per un determinato processo usando **/e: off**. È possibile abilitare o disabilitare le estensioni per tutte le opzioni della riga di comando **cmd** in un computer o una sessione utente impostando i valori di **REG_DWORD** seguenti:

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\EnableExtensions\ REG_DWORD**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\EnableExtensions\ REG_DWORD**

    Impostare il valore di **REG_DWORD** su **0 × 1** (abilitato) o **0 × 0** (disabilitato) nel registro di sistema utilizzando Regedit. exe. Specificato dall'utente e impostazioni avranno precedenza sulle impostazioni del computer e le opzioni della riga di comando hanno la precedenza sulle impostazioni del Registro di sistema.

    > [!CAUTION]
    > La modifica non corretta del Registro di sistema potrebbe danneggiare gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.

    Quando si abilitano le estensioni dei comandi, sono interessati i comandi seguenti:  
    - **assoc**

    - **call**

    - **chdir (CD)**

    - **color**

    - **CANC (Erase)**

    - **endlocal**

    - **for**

    - **ftype**

    - **goto**

    - **if**

    - **mkdir (MD)**

    - **popd**

    - **prompt**

    - **pushd**

    - **set**

    - **setlocal**

    - **shift**

    - **Start** (include anche modifiche ai processi di comando esterni)

- Se si Abilita l'espansione della variabile di ambiente ritardata, è possibile usare il carattere punto esclamativo per sostituire il valore di una variabile di ambiente in fase di esecuzione.

- Il completamento del nome file e directory non è abilitato per impostazione predefinita. È possibile abilitare o disabilitare il completamento del nome file per un particolare processo della **cmd** comando **/f**{**su** | **off**}. È possibile abilitare o disabilitare il completamento del nome file e directory per tutti i processi del comando **cmd** in un computer o per una sessione di accesso utente impostando i valori di **REG_DWORD** seguenti:

    - **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\CompletionChar\REG_DWORD**

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\PathCompletionChar\ REG_DWORD**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\CompletionChar\ REG_DWORD**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\PathCompletionChar\ REG_DWORD**

    Per impostare il valore di **REG_DWORD** , eseguire regedit. exe e usare il valore esadecimale di un carattere di controllo per una funzione specifica (ad esempio, **0 × 9** è Tab e **0 × 08** è Backspace). Specificato dall'utente e impostazioni avranno precedenza sulle impostazioni del computer e le opzioni della riga di comando hanno la precedenza sulle impostazioni del Registro di sistema.

    > [!CAUTION]
    > La modifica non corretta del Registro di sistema potrebbe danneggiare gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.

- Se si Abilita il completamento del nome file e directory usando **/f: on**, usare **CTRL + D** per il completamento del nome di directory e **CTRL + f** per il completamento del nome file. Per disabilitare un carattere di completamento specifico nel Registro di sistema, utilizzare il valore degli spazi vuoti [**0 × 20**] perché non è un carattere di controllo valido.

  - Premendo **CTRL + D** o **CTRL + F**, elabora il completamento del nome file e directory. Queste funzioni di combinazione di tasti aggiungono un carattere jolly a una *stringa* (se non è presente), compila un elenco di percorsi che corrispondono e quindi Visualizza il primo percorso corrispondente.<p>Se nessuno dei percorsi corrispondono, la funzione di completamento di nome file e directory emette un segnale acustico e non modifica la visualizzazione. Per spostarsi nell'elenco dei percorsi corrispondenti, premere ripetutamente **CTRL + D** o **CTRL + F** . Per spostarsi all'indietro nell'elenco, premere il tasto **MAIUSC** e **CTRL + D** o **CTRL + F** simultaneamente. Per eliminare l'elenco di percorsi corrispondenti salvato e generare un nuovo elenco, modificare la *stringa* e premere **CTRL + D** o **CTRL + F**. Se si passa da **CTRL + D** a **CTRL + F**, l'elenco di percorsi corrispondenti salvato viene eliminato e viene generato un nuovo elenco. L'unica differenza tra le combinazioni di tasti **Ctrl + d** e **CTRL + f** è che **Ctrl + d** corrisponde solo ai nomi di directory e **CTRL + f** corrisponde ai nomi di file e directory. Se si utilizza il completamento del nome file e directory in qualsiasi comando di directory incorporato (ovvero **CD**, **MD**o **Rd**), si presuppone il completamento della directory.

  - Il completamento del nome file e directory elabora correttamente i nomi di file che contengono spazi o caratteri speciali, se si inserisce il percorso corrispondente tra virgolette.

  - È necessario racchiudere tra virgolette i seguenti caratteri speciali:  & < >  [] {} ^ =;! '+', ~ [spazio].

  - Se le informazioni fornite contengono spazi, è necessario racchiudere il testo tra virgolette (ad esempio, "nome computer").

  - Se si elabora il completamento del nome file e directory dalla *stringa*, qualsiasi parte del *percorso* a destra del cursore viene scartata (in corrispondenza del punto nella *stringa* in cui è stato elaborato il completamento).

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
