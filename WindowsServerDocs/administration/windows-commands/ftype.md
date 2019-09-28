---
title: ftype
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6fb53cee-9bed-44dd-af5d-bc7cec1dd114
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ce3f4c360269eb9cabd2cbef8abb89935923a595
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375821"
---
# <a name="ftype"></a>ftype



Visualizza o modifica i tipi di file che vengono utilizzati nelle associazioni di estensione nome file. Se utilizzata senza un operatore di assegnazione ( **=** ), **ftype** Visualizza la stringa di comando di apertura corrente per il tipo di file specificato. Se utilizzata senza parametri, **ftype** vengono visualizzati i tipi di file che dispongono di stringhe di comando di apertura definite.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
ftype [<FileType>[=[<OpenCommandString>]]]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<FileType >|Specifica il tipo di file per visualizzare o modificare.|
|\<OpenCommandString >|Specifica la stringa di comando di apertura da usare all'apertura dei file del tipo di file specificato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

La tabella seguente descrive come **ftype** sostituisce le variabili all'interno di una stringa di comando di apertura:

|Variabile|Valore di sostituzione|
|--------|-----------------|
|%1 o %0|Ottiene sostituita con il nome del file viene avviato tramite l'associazione.|
|%*|Ottiene tutti i parametri.|
|% 2,% 3,...|Ottiene il primo parametro (%2), il secondo parametro (%3) e così via.|
|%~ @ NO__T-1N >|Ottiene tutti i parametri rimanenti a partire dal *N*parametro in posizione in cui *N* può essere qualsiasi numero compreso tra 2 e 9.|

## <a name="BKMK_examples"></a>Esempi

Per visualizzare i tipi di file corrente le stringhe di comando di apertura definite, digitare:
```
ftype
```
Per visualizzare la stringa di comando di apertura corrente per il *txtfile* tipo di file, tipo:
```
ftype txtfile
```
Questo comando restituisce un output simile al seguente:
```
txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1
```
Per eliminare la stringa di comando di apertura per un tipo di file denominato *esempio*, tipo:
```
ftype example=
```
Per associare l'estensione pl con il tipo di file PerlScript e abilitare il tipo di file PerlScript eseguire PERL. EXE, digitare i comandi seguenti:
```
assoc .pl=PerlScript 
ftype PerlScript=perl.exe %1 %*
```
Per eliminare la necessità di digitare l'estensione pl quando si richiama uno script Perl, digitare:
```
set PATHEXT=.pl;%PATHEXT%
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)