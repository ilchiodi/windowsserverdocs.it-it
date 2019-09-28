---
title: echo
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fb9fcd0f-5e73-4504-aa95-78204e1a79d3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 343d6327d262401b4be14e472a135062456890f1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377627"
---
# <a name="echo"></a>echo



Visualizza i messaggi o attiva o disattiva la ripetizione dei comandi. Se utilizzata senza parametri, **echo** Visualizza l'impostazione corrente.

Per esempi di utilizzo di questo comando, vedere [Esempi](#examples).

## <a name="syntax"></a>Sintassi

```
echo [<Message>]
echo [on | off]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[in \| off]|Attiva o disattiva la ripetizione dei comandi. Eco dei comandi è abilitata per impostazione predefinita.|
|\<Message >|Specifica il testo da visualizzare sullo schermo.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Il comando **echo** *Message* è particolarmente utile quando **echo** è disattivato. Per visualizzare un messaggio di diverse righe senza visualizzare alcun comando, è possibile includere diversi comandi del *messaggio* Echo dopo il comando **echo off** del programma batch.
-   Quando **echo** è disattivato, il prompt dei comandi non viene visualizzato nella finestra del prompt dei comandi. Per visualizzare il prompt dei comandi, digitare **l'aggiornamento.**
-   Se utilizzato in un file batch, **echo in** e **echo off** non influisce sull'impostazione al prompt dei comandi.
-   Per evitare la ripetizione di un comando specifico in un file batch, inserire un simbolo di chiocciola (@) prima il comando. Per evitare la ripetizione di tutti i comandi in un file batch, includere il **echo off** comando all'inizio del file.
-   Per visualizzare una pipe ( **|** ) o carattere di reindirizzamento ( **<** o **>** ) quando si utilizza **echo**, utilizzare un accento circonflesso (^) immediatamente prima del carattere pipe o reindirizzamento (ad esempio, **^|** , **^>** , o **^<** ). Per visualizzare un punto di inserimento, digitare due accenti circonflessi in successione ( **^^** ).

## <a name="examples"></a>Esempi

Per la visualizzazione corrente **echo** impostazione, digitare:

```
echo
```

Per ripetere una riga vuota nella schermata, digitare:

```
echo.
```

> [!NOTE]
> Non includere uno spazio prima del periodo. In caso contrario, verrà visualizzato il periodo invece di una riga vuota.

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

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
