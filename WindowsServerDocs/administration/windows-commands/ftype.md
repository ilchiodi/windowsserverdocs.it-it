---
title: ftype
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6fb53cee-9bed-44dd-af5d-bc7cec1dd114
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eb4a9fa3105247695f1a50e5fc483ce608cd4816
ms.sourcegitcommit: 7116460855701eed4e09d615693efa4fffc40006
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2020
ms.locfileid: "83433125"
---
# <a name="ftype"></a>ftype



Visualizza o modifica i tipi di file che vengono utilizzati nelle associazioni di estensione nome file. Se usato senza un operatore di assegnazione ( **=** ), **ftype** Visualizza la stringa di comando Open corrente per il tipo di file specificato. Se utilizzata senza parametri, **ftype** vengono visualizzati i tipi di file che dispongono di stringhe di comando di apertura definite.

> [!NOTE]
> Questo comando è supportato solo all'interno di CMD. EXE e non è disponibile da PowerShell.  
> Sebbene sia possibile utilizzare `cmd /c ftype` come soluzione alternativa.


## <a name="syntax"></a>Sintassi

```
ftype [<FileType>[=[<OpenCommandString>]]]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> filetype|Specifica il tipo di file per visualizzare o modificare.|
|\<> OpenCommandString|Specifica la stringa di comando di apertura da usare all'apertura dei file del tipo di file specificato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

La tabella seguente descrive come **ftype** sostituisce le variabili all'interno di una stringa di comando di apertura:

|Variabile|Valore di sostituzione|
|--------|-----------------|
|%1 o %0|Ottiene sostituita con il nome del file viene avviato tramite l'associazione.|
|%*|Ottiene tutti i parametri.|
|%2, %3,...|Ottiene il primo parametro (%2), il secondo parametro (%3) e così via.|
|%~\<N>|Ottiene tutti i parametri rimanenti a partire dal *N*parametro in posizione in cui *N* può essere qualsiasi numero compreso tra 2 e 9.|

## <a name="examples"></a>Esempi

Per visualizzare i tipi di file corrente le stringhe di comando di apertura definite, digitare:
```
ftype
```
Per visualizzare la stringa di comando di apertura corrente per il *txtfile* tipo di file, tipo:
```
ftype txtfile
```
Questo comando consente di generare un output analogo a quello illustrato di seguito.
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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
