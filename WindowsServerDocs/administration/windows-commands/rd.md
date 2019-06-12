---
title: rd
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 42e672f6-5bc2-4c16-af25-18e7ed2dd555
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94231e3ec032280beb91a14db7949a1296c2d811
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442037"
---
# <a name="rd"></a>rd



Elimina una directory. Questo comando è analogo a come le **rmdir** comando.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
rd [<Drive>:]<Path> [/s [/q]]
rmdir [<Drive>:]<Path> [/s [/q]]
```

## <a name="parameters"></a>Parametri

|     Parametro     |                                                                 Descrizione                                                                  |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive>:]<Path> |                      Specifica il percorso e il nome della directory in cui si desidera eliminare. *Percorso* è obbligatorio.                       |
|        /s         |                     Elimina un albero di directory (la directory specificata e tutte le relative sottodirectory, inclusi tutti i file).                      |
|        /q         | Specifica la modalità non interattiva. Non chiedono conferma quando l'eliminazione di un albero di directory. (Si noti che **/q** funziona solo se **/s** è specificato.) |
|        /?         |                                                     Visualizza la guida al prompt dei comandi.                                                     |

## <a name="remarks"></a>Note

-   Non è possibile eliminare una directory che contiene i file, tra cui nascosti o i file di sistema. Se si prova a eseguire questa operazione, viene visualizzato il messaggio seguente:

    `The directory is not empty`

    Usare la **dir /a** comando per elencare tutti i file (tra cui nascosti e i file di sistema). Quindi usare il **attrib** comando **-h** per rimuovere gli attributi di file nascosti, **-s** per rimuovere gli attributi di file system, o **-h -s** da rimuovere sia nascosto e gli attributi di file di sistema. Dopo avere nascosto e gli attributi di file sono stati rimossi, è possibile eliminare i file.
-   Se si inserisce una barra rovesciata (\) all'inizio del *percorso*, *percorso* verrà avviata nella directory radice (indipendentemente dalla directory corrente).
-   Non è possibile usare **rd** per eliminare la directory corrente. Se si prova a eliminare la directory corrente, viene visualizzato il messaggio di errore seguente:

    `The process cannot access the file because it is being used by another process.`

    Se si riceve questo messaggio di errore, è necessario selezionare una directory diversa (non una sottodirectory della directory corrente) e quindi usare **rd** (specificare *percorso* se necessario).
-   Il **rd** comando con parametri diversi, è disponibile dalla Console di ripristino.

## <a name="BKMK_examples"></a>Esempi

È possibile eliminare la directory che sta lavorando. È necessario passare alla directory che non è incluso nella directory corrente. Ad esempio, per passare alla directory padre, digitare:
```
cd ..
```
È ora possibile rimuovere in modo sicuro la directory desiderata.

Usare la **/s** opzione per rimuovere un albero di directory. Ad esempio, per rimuovere una directory denominata Test (e tutte le relative sottodirectory e file) dalla directory corrente, digitare:
```
rd /s test
```
Per eseguire l'esempio precedente in modalità non interattiva, digitare:
```
rd /s /q test
```

> [!CAUTION]
> Quando si esegue **desktop remoto /s** in modalità non interattiva, l'intero albero di directory viene eliminato senza chiedere conferma. Assicurarsi che i file importanti vengono spostati o il backup prima di usare la **/q** opzione della riga di comando.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)