---
title: pushd
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 634dd6dee471751cc62b6899a3963e02e8e783a2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371982"
---
# <a name="pushd"></a>pushd



Archivia la directory corrente per l'uso da parte del comando **popd** , quindi passa alla directory specificata.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
pushd [<Path>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Path >|Specifica la directory in cui eseguire la directory corrente. Questo comando supporta percorsi relativi.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Ogni volta che si usa il comando **pushd** , viene archiviata una singola directory per l'uso. Tuttavia, è possibile archiviare più directory usando il comando **pushd** più volte.

    Le directory vengono archiviate in sequenza in uno stack virtuale. Se si usa il comando **pushd** una sola volta, la directory in cui si usa il comando viene posizionata nella parte inferiore dello stack. Se si usa di nuovo il comando, la seconda directory viene posizionata sopra la prima. Il processo viene ripetuto ogni volta che si usa il comando **pushd** .

    È possibile usare il comando **popd** per impostare la directory corrente sulla directory archiviata più di recente dal comando **pushd** . Se si usa il comando **popd** , la directory nella parte superiore dello stack viene rimossa dallo stack e la directory corrente viene modificata in tale directory. Se si usa di nuovo il comando **popd** , la directory successiva nello stack viene rimossa.
-   Se sono abilitate le estensioni dei comandi, il comando **pushd** accetta un percorso di rete o una lettera e un percorso di unità locale.
-   Se si specifica un percorso di rete, il comando **pushd** assegna temporaneamente la lettera di unità non utilizzata più alta (a partire da Z:) alla risorsa di rete specificata. Il comando modifica quindi l'unità corrente e la directory nella directory specificata nell'unità appena assegnata. Se si usa il comando **popd** con le estensioni del comando abilitate, il comando **popd** rimuove l'assegnazione di lettera di unità creata da **pushd**.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene illustrato come è possibile utilizzare il comando **pushd** e il comando **popd** in un programma batch per modificare la directory corrente da quella in cui è stato eseguito il programma batch e quindi modificarla di nuovo:
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

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Popd](popd.md)