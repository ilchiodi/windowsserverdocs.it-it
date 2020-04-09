---
title: ftype
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6fb53cee-9bed-44dd-af5d-bc7cec1dd114
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f982f68f25a4decbc9c572b533fa1ecc5e893a8c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842704"
---
# <a name="ftype"></a>ftype



Visualizza o modifica i tipi di file che vengono utilizzati nelle associazioni di estensione nome file. Se utilizzata senza un operatore di assegnazione ( **=** ), **ftype** Visualizza la stringa di comando di apertura corrente per il tipo di file specificato. Se utilizzata senza parametri, **ftype** vengono visualizzati i tipi di file che dispongono di stringhe di comando di apertura definite.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
ftype [<FileType>[=[<OpenCommandString>]]]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|> di \<filetype|Specifica il tipo di file per visualizzare o modificare.|
|\<OpenCommandString >|Specifica la stringa di comando di apertura da usare all'apertura dei file del tipo di file specificato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

La tabella seguente descrive come **ftype** sostituisce le variabili all'interno di una stringa di comando di apertura:

|Variable|Valore di sostituzione|
|--------|-----------------|
|%1 o %0|Ottiene sostituita con il nome del file viene avviato tramite l'associazione.|
|%*|Ottiene tutti i parametri.|
|%2, %3,...|Ottiene il primo parametro (%2), il secondo parametro (%3) e così via.|
|%~\<N >|Ottiene tutti i parametri rimanenti a partire dal *N*parametro in posizione in cui *N* può essere qualsiasi numero compreso tra 2 e 9.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)