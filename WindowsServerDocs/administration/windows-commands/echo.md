---
title: echo (eco)
description: Argomento di riferimento per il comando echo, che Visualizza i messaggi o attiva o disattiva la funzionalità di ripetizione dei comandi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fb9fcd0f-5e73-4504-aa95-78204e1a79d3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca2a10a5d52c9d175d453a164f3ab4f47ca0841d
ms.sourcegitcommit: 430c6564c18f89eecb5bbc39cfee1a6f1d8ff85b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/26/2020
ms.locfileid: "83855665"
---
# <a name="echo"></a>echo (eco)

Visualizza i messaggi o attiva o disattiva la ripetizione dei comandi. Se utilizzata senza parametri, **echo** Visualizza l'impostazione corrente.

## <a name="syntax"></a>Sintassi

```
echo [<message>]
echo [on | off]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| [on \| off] | Attiva o disattiva la ripetizione dei comandi. Eco dei comandi è abilitata per impostazione predefinita. |
| `<message>` | Specifica il testo da visualizzare sullo schermo. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Il `echo <message>` comando è particolarmente utile quando **echo** è disattivato. Per visualizzare un messaggio di diverse righe senza visualizzare alcun comando, è possibile includere diversi `echo <message>` comandi dopo il comando **echo off** del programma batch.

- Dopo la disattivazione di **echo** , il prompt dei comandi non viene visualizzato nella finestra del prompt dei comandi. Per visualizzare il prompt dei comandi, digitare **l'aggiornamento.**

- Se usato in un file batch, **echo on** e **echo off** non influiscono sull'impostazione al prompt dei comandi.

- Per evitare che venga restituito un particolare comando in un file batch, inserire un `@` segno di prima del comando. Per evitare la ripetizione di tutti i comandi in un file batch, includere il **echo off** comando all'inizio del file.

- Per visualizzare una barra verticale ( `|` ) o un carattere di reindirizzamento ( `<` o `>` ) quando si utilizza **echo**, utilizzare un accento circonflesso ( `^` ) immediatamente prima della barra verticale o del carattere di reindirizzamento. Ad esempio, `^|` , `^>` o `^<` ). Per visualizzare un punto di inserimento, digitare due accenti circonflessi in successione (`^^`).

### <a name="examples"></a>Esempi

Per la visualizzazione corrente **echo** impostazione, digitare:

```
echo
```

Per ripetere una riga vuota nella schermata, digitare:

```
echo.
```

> [!NOTE]
> Non includere uno spazio prima del periodo. In caso contrario, viene visualizzato il punto anziché una riga vuota.

Per impedire la visualizzazione di comandi al prompt dei comandi, digitare:

```
echo off
```

> [!NOTE]
> Quando **echo** è disattivato, il prompt dei comandi non viene visualizzato nella finestra del prompt dei comandi. Per visualizzare nuovamente il prompt dei comandi, digitare **echo in**.

Per impedire che tutti i comandi in un file batch (incluso il **echo off** comando) da visualizzare sullo schermo, alla prima riga del tipo di file batch:

```
@echo off
```

È possibile utilizzare il **echo** comando come parte di un **Se** istruzione. Ad esempio, per cercare la directory corrente per qualsiasi file con estensione rpt e un messaggio echo se viene trovato uno di questi file, digitare:

```
if exist *.rpt echo The report has arrived.
```

Il seguente file batch cerca nella directory corrente per i file con estensione txt e viene visualizzato un messaggio che indica i risultati della ricerca:

```
@echo off
if not exist *.txt (
echo This directory contains no text files.
) else (
   echo This directory contains the following text files:
   echo.
   dir /b *.txt
   )
```

Se quando viene eseguito il file batch, viene trovato alcun file con estensione txt, viene visualizzato il messaggio seguente:

```
This directory contains no text files.
```

Se vengono trovati file con estensione txt quando viene eseguito il file batch consente di visualizzare l'output seguente (per questo esempio, si supponga che i file File1. txt, file2 e File3.txt esiste):

```
This directory contains the following text files:
File1.txt
File2.txt
File3.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
