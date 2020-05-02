---
title: forfiles
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 43f6b004-446d-4fdd-91c5-5653613524a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 21cbc24028af5c4194d36258aecdd5432fb4069f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725570"
---
# <a name="forfiles"></a>forfiles



Seleziona ed esegue un comando su un file o un set di file. Questo comando è utile per l'elaborazione batch.



## <a name="syntax"></a>Sintassi

```
forfiles [/p <Path>] [/m <SearchMask>] [/s] [/c <Command>] [/d [{+|-}][{<Date>|<Days>}]]
```


### <a name="parameters"></a>Parametri

|                     Parametro                      |                                                                                                                                                                                                                                                                                                    Descrizione                                                                                                                                                                                                                                                                                                     |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /p \<> percorso                     |                                                                                                                                                                                                                                                 Specifica il percorso da cui iniziare la ricerca. Per impostazione predefinita, la ricerca inizia nella directory di lavoro corrente.                                                                                                                                                                                                                                                  |
|                  /m \<SearchMask>                  |                                                                                                                                                                                                                                                           Cerca i file in base alla maschera di ricerca specificati. La maschera di ricerca predefinita ** \*è\\ .** \*.                                                                                                                                                                                                                                                           |
|                         /s                         |                                                                                                                                                                                                                                                                   Indica il **forfiles** comando per la ricerca in modo ricorsivo le sottodirectory.                                                                                                                                                                                                                                                                    |
|                  > \<di comando/c                   |                                                                                                                                                                                                                                  Esegue il comando specificato su ogni file. Stringhe di comando devono essere racchiusa tra virgolette. Il comando predefinito è **cmd/c echo @file **.                                                                                                                                                                                                                                   |
| /d&nbsp;[{+\|-}] &#8288; [{\<date>\|&#8288;\<giorni>}] | Seleziona i file con una data dell'ultima modifica intervallo di tempo specificato.</br>-Seleziona i file con una data dell'Ultima modifica successiva o uguale a**+**() o precedente o uguale a (**-**) la data specificata, in cui la *Data* è nel formato mm/gg/aaaa.</br>-Seleziona i file con una data dell'Ultima modifica successiva o uguale a**+**() la data corrente più il numero di giorni specificato oppure precedente o uguale a (**-**) la data corrente meno il numero di giorni specificato.</br>-I valori validi per *giorni* includere qualsiasi numero compreso nell'intervallo 0-32, 768. Se non viene specificato alcun segno **+** , viene usato per impostazione predefinita. |
|                         /?                         |                                                                                                                                                                                                                                                                                        Visualizza la guida al prompt dei comandi.                                                                                                                                                                                                                                                                                        |

## <a name="remarks"></a>Osservazioni

-   **Forfiles** viene in genere utilizzato nei file batch.
-   **Forfiles /s** è simile a **dir /s.**
-   È possibile utilizzare le seguenti variabili nella stringa di comando come specificato dal **/c** opzione della riga di comando.  

|Variabile|Descrizione|
|--------|-----------|
|@FILE|Nome file.|
|@FNAME|Nome file senza estensione.|
|@EXT|Estensione di file.|
|@PATH|Percorso completo del file.|
|@RELPATH|Percorso relativo del file.|
|@ISDIR|Restituisce TRUE se un tipo di file è una directory. In caso contrario, questa variabile restituisce FALSE.|
|@FSIZE|Dimensione del file in byte.|
|@FDATE|Data ultima modifica al file.|
|@FTIME|Ora ultima modifica al file.|

-   Con **forfiles**, è possibile eseguire un comando in o passare argomenti a più file. Ad esempio, è possibile eseguire il **tipo** comando su tutti i file in una struttura ad albero con l'estensione del nome file con estensione txt. In alternativa, è possibile eseguire ogni file batch (*. bat) nell'unità C, con il nome di file input. txt come primo argomento.
-   Con **forfiles**, è possibile effettuare una delle operazioni seguenti:  
    -   Selezionare i file da una data assoluta o una data relativa utilizzando il **/d** parametro.
    -   Creare una struttura di archiviazione di file usando variabili quali @FSIZE e. @FDATE
    -   Differenziare i file dalle directory usando la @ISDIR variabile.
    -   Includere i caratteri speciali nella riga di comando utilizzando il codice esadecimale del carattere in 0x*HH* formato (ad esempio, per una scheda 0x09).
-   **Forfiles** funziona mediante l'implementazione di **recurse sottodirectory** flag di strumenti progettati per l'elaborazione di un singolo file.

## <a name="examples"></a>Esempi

Per elencare tutti i file batch sull'unità C, digitare:
```
forfiles /p c:\ /s /m *.bat /c cmd /c echo @file is a batch file
```
Per elencare tutte le directory sull'unità C, digitare:
```
forfiles /p c:\ /s /m *.* /c cmd /c if @isdir==TRUE echo @file is a directory
```
Per elencare tutti i file nella directory corrente che sono almeno un anno fa, digitare:
```
forfiles /s /m *.* /d -365 /c cmd /c echo @file is at least one year old.
```
Per visualizzare il *file* di testo obsoleto per ogni file nella directory corrente precedente all'1 gennaio 2007, digitare:
```
forfiles /s /m *.* /d -01/01/2007 /c cmd /c echo @file is outdated. 
```
Per elencare le estensioni di tutti i file nella directory corrente nel formato di colonna e aggiungere una scheda prima dell'estensione, digitare:
```
forfiles /s /m *.* /c cmd /c echo The extension of @file is 0x09@ext 
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
