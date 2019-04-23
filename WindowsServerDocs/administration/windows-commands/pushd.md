---
title: pushd
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 396bc545-0f41-473e-b0ac-76fbbb74d390
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 548f39921c1f6aa3837e6443e396922396eb84f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887462"
---
# <a name="pushd"></a>pushd



Archivia la directory corrente per l'uso dal **popd** comando e quindi modifiche nella directory specificata.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
pushd [<Path>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Path>|Specifica la directory per rendere la directory corrente. Questo comando supporta percorsi relativi.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Ogni volta che usa la **pushd** comando, viene archiviata una singola directory per l'uso. Tuttavia, è possibile archiviare più directory usando il **pushd** comando più volte.

    Le directory vengono archiviate in modo sequenziale in uno stack virtuale. Se si usa la **pushd** comando una volta, la directory in cui è usare il comando viene inserita nella parte inferiore dello stack. Se si usa il comando anche in questo caso, la seconda directory viene inserita all'inizio di quella del primo. Il processo viene ripetuto ogni volta che usa la **pushd** comando.

    È possibile usare la **popd** comando per passare dalla directory corrente alla directory archiviati più di recente per il **pushd** comando. Se si usa la **popd** comando, la directory nella parte superiore dello stack viene rimosso dallo stack e la directory corrente viene modificata in tale directory. Se si usa la **popd** nuovo il comando viene rimossa la directory successiva nello stack.
-   Se sono abilitate le estensioni dei comandi, il **pushd** comando accetta un percorso di rete o una lettera di unità locale e l'unità percorso.
-   Se si specifica un percorso di rete, il **pushd** comando temporaneamente assegna la lettera di unità inutilizzata più alta (a partire da z) per la risorsa di rete specificata. Il comando modifica quindi l'unità corrente e directory nella directory specificata nell'unità appena assegnata. Se si usa la **popd** comando con le estensioni abilitate, il **popd** comando rimuove l'assegnazione di lettera di unità creati da **pushd**.

## <a name="BKMK_examples"></a>Esempi

L'esempio seguente illustra come usare il **pushd** comando e il **popd** comando in un file batch per passare dalla directory corrente da quello in cui è stato eseguito il programma batch e quindi modificarla di nuovo:
```
@echo off
rem This batch file deletes all .txt files in a specified directory
pushd %1
del *.txt
popd
cls
echo All text files deleted in the %1 directory
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

[Popd](popd.md)