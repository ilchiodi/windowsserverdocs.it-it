---
title: forfiles
description: Argomento di riferimento per il comando forfiles, che consente di selezionare ed eseguire un comando in un file o un set di file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 43f6b004-446d-4fdd-91c5-5653613524a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/20/2020
ms.openlocfilehash: 96ef7d016bd13961a4814ba4cd09095aed4f0e97
ms.sourcegitcommit: 29f7a4811b4d36d60b8b7c55ce57d4ee7d52e263
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/20/2020
ms.locfileid: "83716846"
---
# <a name="forfiles"></a>forfiles

Seleziona ed esegue un comando in un file o un set di file. Questo comando è più comunemente usato nei file batch.

## <a name="syntax"></a>Sintassi

```
forfiles [/P pathname] [/M searchmask] [/S] [/C command] [/D [+ | -] [{<date> | <days>}]]
```

### <a name="parameters"></a>Parametri

| Parametro | Description |
| --------- | ----------- |
| /P`<pathname>` | Specifica il percorso da cui iniziare la ricerca. Per impostazione predefinita, la ricerca inizia nella directory di lavoro corrente. |
| /M`<searchmask>` | Cerca i file in base alla maschera di ricerca specificati. Il valore predefinito di searchmask è `*` . |
| /S | Indica al comando **Forfiles** di cercare le sottodirectory in modo ricorsivo. |
| /C`<command>` | Esegue il comando specificato su ogni file. Le stringhe di comando devono essere racchiuse tra virgolette doppie. Il comando predefinito è `"cmd /c echo @file"` . |
| /D`[{+\|-}][{<date> | <days>}]` | Seleziona i file con una data dell'Ultima modifica nell'intervallo di tempo specificato:<ul><li>Seleziona i file con una data dell'Ultima modifica successiva o uguale a ( **+** ) o precedente o uguale a ( **-** ) la data specificata, in cui la *Data* è nel formato mm/gg/aaaa.</li><li>Seleziona i file con una data dell'Ultima modifica successiva o uguale a ( **+** ) la data corrente più il numero di giorni specificato oppure precedente o uguale a ( **-** ) la data corrente meno il numero di giorni specificato.</li><li>I valori validi per i *giorni* includono qualsiasi numero nell'intervallo compreso tra 0 e 32768. Se non viene specificato alcun segno, **+** viene usato per impostazione predefinita.</li></ul> |
| /? | Consente di visualizzare il testo della guida nella finestra cmd. |

#### <a name="remarks"></a>Osservazioni

- Il `forfiles /S` comando è simile a `dir /S` .

- È possibile utilizzare le variabili seguenti nella stringa di comando come specificato dall'opzione della riga di comando **/c** :

    | Variabile | Descrizione |
    | -------- | ----------- |
    | @FILE | Nome file. |
    | @FNAME | Nome file senza estensione. |
    | @EXT | Estensione di file. |
    | @PATH | Percorso completo del file. |
    | @RELPATH | Percorso relativo del file. |
    | @ISDIR | Restituisce TRUE se un tipo di file è una directory. In caso contrario, questa variabile restituisce FALSE. |
    | @FSIZE | Dimensione del file in byte. |
    | @FDATE | Data ultima modifica al file. |
    | @FTIME | Ora ultima modifica al file. |

- Il comando **Forfiles** consente di eseguire un comando o di passare argomenti a più file. Ad esempio, è possibile eseguire il **tipo** comando su tutti i file in una struttura ad albero con l'estensione del nome file con estensione txt. In alternativa, è possibile eseguire ogni file batch (*. bat) nell'unità C, con il nome di file input. txt come primo argomento.

- Questo comando può:

    - Selezionare i file da una data assoluta o una data relativa utilizzando il **/d** parametro.

    - Creare una struttura di archiviazione di file usando variabili quali @FSIZE e @FDATE .

    - Differenziare i file dalle directory usando la @ISDIR variabile.

    - Includere i caratteri speciali nella riga di comando utilizzando il codice esadecimale del carattere in 0x*HH* formato (ad esempio, per una scheda 0x09).

- Questo comando funziona implementando il `recurse subdirectories` flag sugli strumenti progettati per elaborare un singolo file.

### <a name="examples"></a>Esempi

Per elencare tutti i file batch sull'unità C, digitare:

```
forfiles /P c:\ /S /M *.bat /C "cmd /c echo @file is a batch file"
```

Per elencare tutte le directory sull'unità C, digitare:

```
forfiles /P c:\ /S /M *.* /C "cmd /c if @isdir==TRUE echo @file is a directory"
```

Per elencare tutti i file nella directory corrente che sono almeno un anno fa, digitare:

```
forfiles /S /M *.* /D -365 /C "cmd /c echo @file is at least one year old."
```

Per visualizzare il *file* di testo obsoleto per ogni file nella directory corrente precedente all'1 gennaio 2007, digitare:

```
forfiles /S /M *.* /D -01/01/2007 /C "cmd /c echo @file is outdated."
```

Per elencare le estensioni di tutti i file nella directory corrente nel formato di colonna e aggiungere una scheda prima dell'estensione, digitare:

```
forfiles /S /M *.* /C "cmd /c echo The extension of @file is 0x09@ext"
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
