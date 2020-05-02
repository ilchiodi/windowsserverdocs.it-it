---
title: popd
description: Informazioni su come modificare la directory nella directory archiviata più di recente dal comando pushd.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8a4c52d5-9fd1-4eac-9c0c-5767b25728ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: a1638f8d0d62b730578cb21fac5a74e58ad06b74
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723291"
---
# <a name="popd"></a>popd

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta la directory corrente sulla directory archiviata più di recente dal comando **pushd** .


## <a name="syntax"></a>Sintassi
```
popd
```

#### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni
-   Ogni volta che si usa il comando **pushd** , viene archiviata una singola directory per l'uso. Tuttavia, è possibile archiviare più directory usando il comando **pushd** più volte.
    Le directory vengono archiviate in sequenza in uno stack virtuale. Se si usa il comando **pushd** una sola volta, la directory in cui si usa il comando viene posizionata nella parte inferiore dello stack. Se si usa di nuovo il comando, la seconda directory viene posizionata sopra la prima. Il processo viene ripetuto ogni volta che si usa il comando **pushd** .
    È possibile usare il comando **popd** per impostare la directory corrente sulla directory archiviata più di recente dal comando **pushd** . Se si usa il comando **popd** , la directory nella parte superiore dello stack viene rimossa dallo stack e la directory corrente viene modificata in tale directory. Se si usa di nuovo il comando **popd** , la directory successiva nello stack viene rimossa.
-   Quando sono abilitate le estensioni dei comandi, il comando **popd** rimuove le assegnazioni di lettera di unità create da **pushd**.

## <a name="examples"></a><a name="BKMK_examples"></a>Esempi
Per illustrare come è possibile usare il comando **pushd** e il comando **popd** in un programma batch per modificare la directory corrente da quella in cui è stato eseguito il programma batch e quindi modificarlo di nuovo:

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
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

