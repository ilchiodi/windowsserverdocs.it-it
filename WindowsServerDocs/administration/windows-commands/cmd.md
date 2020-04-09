---
title: cmd
description: Windows Commands Topic for cmd, che avvia una nuova istanza dell'interprete dei comandi, cmd. exe.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ec588db-31a9-4a73-a970-65a2c6f4abbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 83b5e27017a9a0f979acec428b8ddaa73cd9d46b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847624"
---
# <a name="cmd"></a>cmd

Avvia una nuova istanza dell'interprete dei comandi, Cmd.exe. Se utilizzata senza parametri, **cmd** vengono visualizzate le informazioni sul copyright e versione del sistema operativo.

## <a name="syntax"></a>Sintassi

```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<B><F>|<F>}] [/e:{on|off}] [/f:{on|off}] [/v:{on|off}] [<String>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/c|Esegue il comando specificato da *stringa* e quindi si arresta.|
|/k|Esegue il comando specificato da *stringa* e continua.|
|/s|Modifica il trattamento della *stringa* dopo **/c** o **/k**.|
|/q|Disattiva l'eco.|
|/d|Disattiva l'esecuzione dei comandi di esecuzione automatica.|
|/a|Formatta l'output del comando interno a una pipe o un file come American National Standards Institute (ANSI).|
|/u|Formatta l'output del comando interno a una pipe o un file come Unicode.|
|/t: {\<B\>\<F\>\|\<F\>}|Imposta lo sfondo (*B*) e di primo piano (*F*) i colori.|
|/e: in|Attiva le estensioni del comando.|
|/e: off|Disabilita le estensioni ai comandi.|
|/f: in|Consente il completamento del nome file e directory.|
|/f: off|Disabilita il completamento del nome file e directory.|
|/v: in|Consente di ritardare l'espansione della variabile di ambiente.|
|/v: off|Disabilita ritardata espansione della variabile di ambiente.|
|\<stringa >|Specifica il comando che si desidera eseguire.|
|/?|Visualizza la guida al prompt dei comandi.|

Nella tabella seguente sono elencate le cifre esadecimali valide che è possibile utilizzare come valori per \<B\> e \<F\>

|Valore|Colore|
|-----|-----|
|0|nero|
|1|Blue|
|2|Verde|
|3|Verde acqua|
|4|Rosso|
|5|Purple|
|6|Yellow|
|7|Vuoto|
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

    Per usare più comandi per \<stringa >, separarli dal separatore di comandi **&&** e racchiuderli tra virgolette. Ad esempio,

    ```
    <Command>&&<Command>&&<Command>
    ``` 
 
-   Elaborazione virgolette

    Se si specifica **/c** o **/k**, **cmd** elabora il resto della *stringa* e le virgolette vengono mantenute solo se vengono soddisfatte tutte le condizioni seguenti:  
    -   Non si utilizza **/s**.
    -   Utilizzare un unico set di virgolette.
    -   Non utilizzare caratteri speciali all'interno delle virgolette (ad esempio: & () < > @ ^ |).
    -   Utilizzare uno o più caratteri spazio vuoto tra virgolette.
    -   Il *stringa* racchiuso tra virgolette è il nome di un file eseguibile.

    Se non sono soddisfatte le condizioni precedenti, *stringa* viene elaborata esaminando il primo carattere per verificare se si tratta di un segno di virgolette di apertura. Se il primo carattere è un segno di virgolette di apertura, si è rimosso insieme virgolette di chiusura. Qualsiasi testo che segue le virgolette di chiusura viene mantenuto.
-   Esecuzione di sottochiavi del registro di sistema

    Se non si specifica **/d** nella *stringa*, cmd. exe cercherà le sottochiavi del registro di sistema seguenti:

    **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\AutoRun\ REG_SZ**

    **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\AutoRun\ REG_EXPAND_SZ**

    Se sono presenti una o entrambe le sottochiavi del registro di sistema, vengono eseguite prima di tutte le altre variabili.

> [!CAUTION]
> È possibile che eventuali modifiche non corrette del Registro di sistema danneggino gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.

-   Abilitazione e disabilitazione delle estensioni del comando

    Le estensioni dei comandi sono abilitate per impostazione predefinita in Windows XP. È possibile disabilitarli per un determinato processo utilizzando **/e: off**. È possibile abilitare o disabilitare le estensioni per tutte le opzioni della riga di comando **cmd** in un computer o una sessione utente impostando i valori di **REG_DWORD** seguenti:

    **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\EnableExtensions\ REG_DWORD**

    **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\EnableExtensions\ REG_DWORD**

    Impostare il valore di **REG_DWORD** su **0 × 1** (abilitato) o **0 × 0** (disabilitato) nel registro di sistema utilizzando Regedit. exe. Specificato dall'utente e impostazioni avranno precedenza sulle impostazioni del computer e le opzioni della riga di comando hanno la precedenza sulle impostazioni del Registro di sistema.

> [!CAUTION]
> È possibile che eventuali modifiche non corrette del Registro di sistema danneggino gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.

    When you enable command extensions, the following commands are affected:  
    -  **assoc**
    -  **call**
    -  **chdir (cd)**
    -  **color**
    -  **del (erase)**
    -  **endlocal**
    -  **for**
    -  **ftype**
    -  **goto**
    -  **if**
    -  **mkdir (md)**
    -  **popd**
    -  **prompt**
    -  **pushd**
    -  **set**
    -  **setlocal**
    -  **shift**
    -  **start** (also includes changes to external command processes)

-   Abilitazione dell'espansione delle variabili di ambiente ritardata

    Se si Abilita l'espansione della variabile di ambiente ritardata, è possibile usare il carattere punto esclamativo per sostituire il valore di una variabile di ambiente in fase di esecuzione.
-   Abilitazione del completamento del nome file e directory

    Il completamento del nome file e directory non è abilitato per impostazione predefinita. È possibile abilitare o disabilitare il completamento del nome file per un particolare processo della **cmd** comando **/f**{**su**|**off**}. È possibile abilitare o disabilitare il completamento del nome file e directory per tutti i processi del comando **cmd** in un computer o per una sessione di accesso utente impostando i valori di **REG_DWORD** seguenti:

    **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\CompletionChar\ REG_DWORD**

    **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\PathCompletionChar\ REG_DWORD**

    **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\CompletionChar\ REG_DWORD**

    **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\PathCompletionChar\ REG_DWORD**

    Per impostare il valore di **REG_DWORD** , eseguire regedit. exe e usare il valore esadecimale di un carattere di controllo per una funzione specifica (ad esempio, **0 × 9** è Tab e **0 × 08** è Backspace). Specificato dall'utente e impostazioni avranno precedenza sulle impostazioni del computer e le opzioni della riga di comando hanno la precedenza sulle impostazioni del Registro di sistema.

> [!CAUTION]
> È possibile che eventuali modifiche non corrette del Registro di sistema danneggino gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.

Se si abilita il completamento del nome file e directory tramite **/f: su**, utilizzare CTRL + D per il completamento del nome di directory e CTRL + F per il completamento del nome file. Per disabilitare un carattere di completamento specifico nel Registro di sistema, utilizzare il valore degli spazi vuoti [**0 × 20**] perché non è un carattere di controllo valido.

Quando si preme CTRL + D o CTRL + F, **cmd** Elabora il completamento del nome file e directory. Queste funzioni di combinazione di tasti aggiungono un carattere jolly per *stringa* (se non è presente), compilare un elenco di percorsi corrispondenti e quindi visualizzare il primo percorso corrispondente. Se nessuno dei percorsi corrispondono, la funzione di completamento di nome file e directory emette un segnale acustico e non modifica la visualizzazione. Per spostarsi nell'elenco di percorsi corrispondenti, premere ripetutamente CTRL + D o CTRL + F. Per spostarsi all'indietro nell'elenco, premere il tasto MAIUSC e CTRL + D o CTRL + F contemporaneamente. Per eliminare l'elenco di percorsi corrispondenti salvato e generare un nuovo elenco, modificare *stringa* e premere CTRL + D o CTRL + F. Se si passa da CTRL + D e CTRL + F, viene eliminato l'elenco di percorsi corrispondenti salvato e viene generato un nuovo elenco. L'unica differenza tra le combinazioni di tasti CTRL + D e CTRL + F è che CTRL + D corrisponde solo i nomi di directory e CTRL + F corrisponde a nomi di file e di directory. Se si utilizza il completamento del nome file e directory in qualsiasi comando di directory incorporato (ovvero **CD**, **MD**o **Rd**), si presuppone il completamento della directory.

Il completamento del nome file e directory elabora correttamente i nomi di file che contengono spazi o caratteri speciali, se si inserisce il percorso corrispondente tra virgolette.

I caratteri speciali seguenti richiedono le virgolette: & < > [] {} ^ =;! '+', ~ [spazio].

Se le informazioni fornite contengono spazi, racchiudere il testo tra virgolette, ad esempio il nome del computer.

Se si elabora il completamento del nome file e directory all'interno *stringa*, qualsiasi parte il *percorso* a destra del cursore viene scartato (nel punto *stringa* in cui è stato elaborato il completamento).

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
