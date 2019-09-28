---
title: forfiles
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 43f6b004-446d-4fdd-91c5-5653613524a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: fa2eb5c96dfbf3870705af41a0f27991084f816d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377073"
---
# <a name="forfiles"></a>forfiles



Seleziona ed esegue un comando su un file o un set di file. Questo comando è utile per l'elaborazione batch.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
forfiles [/p <Path>] [/m <SearchMask>] [/s] [/c "<Command>"] [/d [{+|-}][{<Date>|<Days>}]]
```


## <a name="parameters"></a>Parametri

|                     Parametro                      |                                                                                                                                                                                                                                                                                                    Descrizione                                                                                                                                                                                                                                                                                                     |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /p \<Path >                     |                                                                                                                                                                                                                                                 Specifica il percorso da cui iniziare la ricerca. Per impostazione predefinita, la ricerca inizia nella directory di lavoro corrente.                                                                                                                                                                                                                                                  |
|                  /m \<SearchMask >                  |                                                                                                                                                                                                                                                           Cerca i file in base alla maschera di ricerca specificati. La maschera di ricerca predefinita è **\*. \\** \*.                                                                                                                                                                                                                                                           |
|                         /s                         |                                                                                                                                                                                                                                                                   Indica il **forfiles** comando per la ricerca in modo ricorsivo le sottodirectory.                                                                                                                                                                                                                                                                    |
|                  /c "\<Command >"                   |                                                                                                                                                                                                                                  Esegue il comando specificato su ogni file. Stringhe di comando devono essere racchiusa tra virgolette. Il comando predefinito è **"cmd/c echo @file"** .                                                                                                                                                                                                                                   |
| /d @ no__t-0 [{+ \|-}]&#8288;[{\<Date > \|&#8288;\<days >}] | Seleziona i file con una data dell'ultima modifica intervallo di tempo specificato.</br>-Consente di selezionare i file con una data dell'ultima modifica successiva o uguale a ( **+** ) o precedente o uguale a ( **-** ) la data specificata, in cui *Data* è nel formato MM/GG/AAAA.</br>-Consente di selezionare i file con una data dell'ultima modifica successiva o uguale a ( **+** ) la data corrente più il numero di giorni specificati, o precedente o uguale a ( **-** ) la data corrente meno il numero di giorni specificato.</br>-I valori validi per *giorni* includere qualsiasi numero compreso nell'intervallo 0-32, 768. Se non viene specificato alcun segno, **+** viene utilizzato per impostazione predefinita. |
|                         /?                         |                                                                                                                                                                                                                                                                                        Visualizza la guida al prompt dei comandi.                                                                                                                                                                                                                                                                                        |

## <a name="remarks"></a>Note

-   **Forfiles** viene in genere utilizzato nei file batch.
-   **Forfiles /s** è simile a **dir /s.**
-   È possibile utilizzare le seguenti variabili nella stringa di comando come specificato dal **/c** opzione della riga di comando.  

|Variabile|Descrizione|
|--------|-----------|
|@FILE|Nome del file.|
|@FNAME|Nome file senza estensione.|
|@EXT|Estensione di file.|
|@PATH|Percorso completo del file.|
|@RELPATH|Percorso relativo del file.|
|@ISDIR|Restituisce TRUE se un tipo di file è una directory. In caso contrario, questa variabile restituisce FALSE.|
|@FSIZE|Dimensione del file in byte.|
|@FDATE|Data ultima modifica al file.|
|@FTIME|Ora ultima modifica al file.|

-   Con **forfiles**, è possibile eseguire un comando in o passare argomenti a più file. Ad esempio, è possibile eseguire il **tipo** comando su tutti i file in una struttura ad albero con l'estensione del nome file con estensione txt. Oppure è possibile eseguire tutti i file batch (*. bat) sull'unità C, con il nome file "Mioinput. txt" come primo argomento.
-   Con **forfiles**, è possibile effettuare una delle operazioni seguenti:  
    -   Selezionare i file da una data assoluta o una data relativa utilizzando il **/d** parametro.
    -   Creare una struttura di archiviazione di file usando variabili quali @FSIZE e @FDATE.
    -   Differenziare i file dalle directory usando la variabile @ISDIR.
    -   Includere i caratteri speciali nella riga di comando utilizzando il codice esadecimale del carattere in 0x*HH* formato (ad esempio, per una scheda 0x09).
-   **Forfiles** funziona mediante l'implementazione di **recurse sottodirectory** flag di strumenti progettati per l'elaborazione di un singolo file.

## <a name="BKMK_examples"></a>Esempi

Per elencare tutti i file batch sull'unità C, digitare:
```
forfiles /p c:\ /s /m *.bat /c "cmd /c echo @file is a batch file"
```
Per elencare tutte le directory sull'unità C, digitare:
```
forfiles /p c:\ /s /m *.* /c "cmd /c if @isdir==TRUE echo @file is a directory"
```
Per elencare tutti i file nella directory corrente che sono almeno un anno fa, digitare:
```
forfiles /s /m *.* /d -365 /c "cmd /c echo @file is at least one year old."
```
Per visualizzare il testo "*File* è obsoleto" per ogni file nella directory corrente antecedenti il 1 ° gennaio 2007, digitare:
```
forfiles /s /m *.* /d -01/01/2007 /c "cmd /c echo @file is outdated." 
```
Per elencare le estensioni di tutti i file nella directory corrente nel formato di colonna e aggiungere una scheda prima dell'estensione, digitare:
```
forfiles /s /m *.* /c "cmd /c echo The extension of @file is 0x09@ext" 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
