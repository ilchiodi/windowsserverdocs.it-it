---
title: Cmd
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ec588db-31a9-4a73-a970-65a2c6f4abbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 966ac7f70984dff6d26265e07a26a6eebcde9fb6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874392"
---
# <a name="cmd"></a>Cmd



Avvia una nuova istanza dell'interprete dei comandi, Cmd.exe. Se utilizzata senza parametri, **cmd** vengono visualizzate le informazioni sul copyright e versione del sistema operativo.

## <a name="syntax"></a>Sintassi

```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<B><F>|<F>}] [/e:{on|off}] [/f:{on|off}] [/v:{on|off}] [<String>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/c|Esegue il comando specificato da *stringa* e quindi si arresta.|
|/k|Esegue il comando specificato da *stringa* e continua.|
|/s|Modifica il trattamento delle *stringa* dopo **/c** oppure **/k**.|
|/q|Disattiva l'eco.|
|/d|Disattiva l'esecuzione dei comandi di esecuzione automatica.|
|/a|Formatta l'output del comando interno a una pipe o un file come American National Standards Institute (ANSI).|
|/u|Formatta l'output del comando interno a una pipe o un file come Unicode.|
|/t:{\<B\>\<F\>\|\<F\>}|Imposta lo sfondo (*B*) e di primo piano (*F*) i colori.|
|/e: in|Attiva le estensioni del comando.|
|/e: off|Disabilita le estensioni ai comandi.|
|/f: in|Consente il completamento del nome file e directory.|
|/f: off|Disabilita il completamento del nome file e directory.|
|/v: in|Consente di ritardare l'espansione della variabile di ambiente.|
|/v: off|Disabilita ritardata espansione della variabile di ambiente.|
|\<String>|Specifica il comando che si desidera eseguire.|
|/?|Visualizza la guida al prompt dei comandi.|

Nella tabella seguente sono elencate le cifre esadecimali valide che è possibile utilizzare come valori per \<B\> e \<F\>

|Value|Colore|
|-----|-----|
|0|Nero|
|1|Blu|
|2|Verde|
|3|Aqua|
|4|Rossa|
|5|Viola|
|6|Giallo|
|7|Bianco|
|8|Grigio|
|9|Azzurro|
|a|Verde chiaro|
|b|Azzurro|
|c|Rosso|
|d|Viola chiaro|
|e|Giallo|
|f|Sfondo bianco|

## <a name="remarks"></a>Note

-   Uso di più comandi

    Per utilizzare più comandi per \<String >, è necessario separarli con il separatore di comandi **&&** e racchiuderle tra virgolette. Ad esempio:  
    ```
    "<Command>&&<Command>&&<Command>"
    ```  
-   L'elaborazione tra virgolette

    Se si specifica **/c** o **/k**, **cmd** elabora la parte restante di *stringa,* e racchiusi tra virgolette vengono mantenute solo se tutti i valori sono i seguenti le condizioni sono soddisfatte:  
    -   Non si utilizza **/s**.
    -   Utilizzare un unico set di virgolette.
    -   Non utilizzare caratteri speciali all'interno delle virgolette (ad esempio: & () < > @ ^ |).
    -   Utilizzare uno o più caratteri spazio vuoto tra virgolette.
    -   Il *stringa* racchiuso tra virgolette è il nome di un file eseguibile.

    Se non sono soddisfatte le condizioni precedenti, *stringa* viene elaborata esaminando il primo carattere per verificare se si tratta di un segno di virgolette di apertura. Se il primo carattere è un segno di virgolette di apertura, si è rimosso insieme virgolette di chiusura. Qualsiasi testo che segue le virgolette di chiusura viene mantenuto.
-   Sottochiavi del Registro di sistema in esecuzione

    Se non si specifica **/d** nelle *stringa*, Cmd.exe cerca le sottochiavi del Registro di sistema seguenti:

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\AutoRun\REG_SZ**

    **Processor\autorun\reg_expand_sz HKEY_CURRENT_USER\Software\Microsoft\Command**

    Se una o entrambe le sottochiavi del Registro di sistema sono presenti, vengono eseguiti prima delle altre variabili.

> [!CAUTION]
> La modifica non corretta del Registro di sistema potrebbe danneggiare gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.
-   Abilitazione e disabilitazione delle estensioni di comando

    Le estensioni comando sono abilitate per impostazione predefinita in Windows XP. È possibile disabilitarli per un determinato processo utilizzando **/e: off**. È possibile abilitare o disabilitare le estensioni per tutti i **cmd** le opzioni della riga di comando in una sessione utente o computer impostando gli elementi seguenti **REG_DWORD** valori:

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\EnableExtensions\REG_DWORD**

    **Processor\EnableExtensions\REG_DWORD HKEY_CURRENT_USER\Software\Microsoft\Command**

    Impostare il **REG_DWORD** valore a una delle due **0 × 1** (abilitato) o **0 × 0** (disabilitato) nel Registro di sistema usando Regedit.exe. Specificato dall'utente e impostazioni avranno precedenza sulle impostazioni del computer e le opzioni della riga di comando hanno la precedenza sulle impostazioni del Registro di sistema.

>     [!CAUTION]
>     Incorrectly editing the registry may severely damage your system. Before making changes to the registry, you should back up any valued data on the computer.

    When you enable command extensions, the following commands are affected:  
    -   **assoc**
    -   **call**
    -   **chdir (cd)**
    -   **color**
    -   **del (erase)**
    -   **endlocal**
    -   **for**
    -   **ftype**
    -   **goto**
    -   **if**
    -   **mkdir (md)**
    -   **popd**
    -   **prompt**
    -   **pushd**
    -   **set**
    -   **setlocal**
    -   **shift**
    -   **start** (also includes changes to external command processes)
-   Abilitazione di espansione della variabile di ambiente ritardata

    Se si abilita espansione della variabile di ambiente ritardata, è possibile utilizzare il carattere punto esclamativo per sostituire il valore di una variabile di ambiente in fase di esecuzione.
-   Abilitazione di completamento del nome file e directory

    Il completamento del nome file e directory non è abilitato per impostazione predefinita. È possibile abilitare o disabilitare il completamento del nome file per un particolare processo della **cmd** comando **/f**{**su**|**off**}. È possibile abilitare o disabilitare il completamento del nome file e directory per tutti i processi dei **cmd** comando in un computer o per una sessione di accesso utente impostando gli elementi seguenti **REG_DWORD** valori:

    **Processor\CompletionChar\REG_DWORD HKEY_LOCAL_MACHINE\Software\Microsoft\Command**

    **Processor\PathCompletionChar\REG_DWORD HKEY_LOCAL_MACHINE\Software\Microsoft\Command**

    **Processor\autorun\reg_expand_sz Processor\CompletionChar\REG_DWORD**

    **Processor\autorun\reg_expand_sz Processor\PathCompletionChar\REG_DWORD**

    Per impostare il **REG_DWORD** valore, eseguire Regedit.exe e utilizzare il valore esadecimale del carattere di controllo per una determinata funzione (ad esempio **0 × 9** scheda e **0 × 08** è BACKSPACE). Specificato dall'utente e impostazioni avranno precedenza sulle impostazioni del computer e le opzioni della riga di comando hanno la precedenza sulle impostazioni del Registro di sistema.

> [!CAUTION]
> La modifica non corretta del Registro di sistema potrebbe danneggiare gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.

Se si abilita il completamento del nome file e directory tramite **/f: su**, utilizzare CTRL + D per il completamento del nome di directory e CTRL + F per il completamento del nome file. Per disabilitare un carattere di completamento specifico nel Registro di sistema, utilizzare il valore degli spazi vuoti [**0 × 20**] perché non è un carattere di controllo valido.

Quando si preme CTRL + D o CTRL + F, **cmd** Elabora il completamento del nome file e directory. Queste funzioni di combinazione di tasti aggiungono un carattere jolly per *stringa* (se non è presente), compilare un elenco di percorsi corrispondenti e quindi visualizzare il primo percorso corrispondente. Se nessuno dei percorsi corrispondono, la funzione di completamento di nome file e directory emette un segnale acustico e non modifica la visualizzazione. Per spostarsi nell'elenco di percorsi corrispondenti, premere ripetutamente CTRL + D o CTRL + F. Per spostarsi all'indietro nell'elenco, premere il tasto MAIUSC e CTRL + D o CTRL + F contemporaneamente. Per eliminare l'elenco di percorsi corrispondenti salvato e generare un nuovo elenco, modificare *stringa* e premere CTRL + D o CTRL + F. Se si passa da CTRL + D e CTRL + F, viene eliminato l'elenco di percorsi corrispondenti salvato e viene generato un nuovo elenco. L'unica differenza tra le combinazioni di tasti CTRL + D e CTRL + F è che CTRL + D corrisponde solo i nomi di directory e CTRL + F corrisponde a nomi di file e di directory. Se si usa il completamento del nome file e directory in uno dei comandi directory incorporata (vale a dire **CD**, **MD**, o **desktop remoto**), si presuppone che il completamento di directory.

Il completamento del nome file e directory elabora correttamente i nomi di file che contengono spazi o caratteri speciali, se si inserisce il percorso corrispondente tra virgolette.

I caratteri speciali seguenti richiedono le virgolette: & < > [] {} ^ =;! '+', ~ [spazio].

Se le informazioni che viene fornito contengono spazi, utilizzare le virgolette intorno al testo (ad esempio, "nome Computer").

Se si elabora il completamento del nome file e directory all'interno *stringa*, qualsiasi parte il *percorso* a destra del cursore viene scartato (nel punto *stringa* in cui è stato elaborato il completamento).

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
