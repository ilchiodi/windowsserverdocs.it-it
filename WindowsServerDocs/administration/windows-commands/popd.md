---
title: popd
description: Informazioni su come modificare la directory della directory archiviati più di recente dal comando pushd.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a4c52d5-9fd1-4eac-9c0c-5767b25728ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6da6dc9d1fc2d8965f8a081831cb1150375209a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827462"
---
# <a name="popd"></a>popd

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica la directory corrente alla directory in cui più di recente è stata archiviata per il **pushd** comando.
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi
```
popd
```

### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   Ogni volta che usa la **pushd** comando, viene archiviata una singola directory per l'uso. Tuttavia, è possibile archiviare più directory usando il **pushd** comando più volte.
    Le directory vengono archiviate in modo sequenziale in uno stack virtuale. Se si usa la **pushd** comando una volta, la directory in cui è usare il comando viene inserita nella parte inferiore dello stack. Se si usa il comando anche in questo caso, la seconda directory viene inserita all'inizio di quella del primo. Il processo viene ripetuto ogni volta che usa la **pushd** comando.
    È possibile usare la **popd** comando per passare dalla directory corrente alla directory archiviati più di recente per il **pushd** comando. Se si usa la **popd** comando, la directory nella parte superiore dello stack viene rimosso dallo stack e la directory corrente viene modificata in tale directory. Se si usa la **popd** nuovo il comando viene rimossa la directory successiva nello stack.
-   Quando sono abilitate le estensioni dei comandi, il **popd** comando rimuove le assegnazioni di qualsiasi lettera di unità create dal **pushd**.

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

## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [pushd](pushd.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)

