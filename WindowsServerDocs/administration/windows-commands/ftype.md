---
title: ftype
description: Argomento di riferimento per il comando ftype, che consente di visualizzare o modificare il tipo di file utilizzato nelle associazioni di estensione del nome file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6fb53cee-9bed-44dd-af5d-bc7cec1dd114
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1387a9f8cb607d3563a381c757ea237104e6032
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820211"
---
# <a name="ftype"></a>ftype

Visualizza o modifica i tipi di file che vengono utilizzati nelle associazioni di estensione nome file. Se usato senza un operatore di assegnazione (=), questo comando Visualizza la stringa di comando Open corrente per il tipo di file specificato. Se usato senza parametri, questo comando Visualizza i tipi di file con stringhe di comando aperte definite.

> [!NOTE]
> Questo comando è supportato solo in cmd. exe e non è disponibile da PowerShell.
> Sebbene sia possibile utilizzare `cmd /c ftype` come soluzione alternativa.

## <a name="syntax"></a>Sintassi

```
ftype [<filetype>[=[<opencommandstring>]]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<filetype>` | Specifica il tipo di file per visualizzare o modificare. |
| `<opencommandstring>` | Specifica la stringa di comando di apertura da usare all'apertura dei file del tipo di file specificato.|
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

La tabella seguente descrive come **ftype** sostituisce le variabili all'interno di una stringa di comando di apertura:

| Variabile | Valore di sostituzione |
| -------- | ----------------- |
| `%0` o `%1` | Ottiene sostituita con il nome del file viene avviato tramite l'associazione. |
| `%*` | Ottiene tutti i parametri. |
| `%2`, `%3`, ... | Ottiene il primo parametro ( `%2` ), il secondo parametro ( `%3` ) e così via. |
| `%~<n>` | Ottiene tutti i parametri rimanenti che iniziano con il *n*parametro, dove *n* può essere qualsiasi numero compreso tra 2 e 9. |

### <a name="examples"></a>Esempi

Per visualizzare i tipi di file corrente le stringhe di comando di apertura definite, digitare:

```
ftype
```

Per visualizzare la stringa di comando di apertura corrente per il *txtfile* tipo di file, tipo:

```
ftype txtfile
```

Questo comando consente di generare un output analogo a quello illustrato di seguito.

`txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1`

Per eliminare la stringa di comando Open per un tipo di file denominato *example*, digitare:

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
