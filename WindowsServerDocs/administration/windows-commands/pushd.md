---
title: pushd
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 396bc545-0f41-473e-b0ac-76fbbb74d390
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e64c4f5090183b7d7b29dc7e040ffd94dc9d57ce
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722766"
---
# <a name="pushd"></a>pushd



Archivia la directory corrente per l'uso da parte del comando **popd** , quindi passa alla directory specificata.



## <a name="syntax"></a>Sintassi

```
pushd [<Path>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> percorso|Specifica la directory in cui eseguire la directory corrente. Questo comando supporta percorsi relativi.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   Ogni volta che si usa il comando **pushd** , viene archiviata una singola directory per l'uso. Tuttavia, è possibile archiviare più directory usando il comando **pushd** più volte.

    Le directory vengono archiviate in sequenza in uno stack virtuale. Se si usa il comando **pushd** una sola volta, la directory in cui si usa il comando viene posizionata nella parte inferiore dello stack. Se si usa di nuovo il comando, la seconda directory viene posizionata sopra la prima. Il processo viene ripetuto ogni volta che si usa il comando **pushd** .

    È possibile usare il comando **popd** per impostare la directory corrente sulla directory archiviata più di recente dal comando **pushd** . Se si usa il comando **popd** , la directory nella parte superiore dello stack viene rimossa dallo stack e la directory corrente viene modificata in tale directory. Se si usa di nuovo il comando **popd** , la directory successiva nello stack viene rimossa.
-   Se sono abilitate le estensioni dei comandi, il comando **pushd** accetta un percorso di rete o una lettera e un percorso di unità locale.
-   Se si specifica un percorso di rete, il comando **pushd** assegna temporaneamente la lettera di unità non utilizzata più alta (a partire da Z:) alla risorsa di rete specificata. Il comando modifica quindi l'unità corrente e la directory nella directory specificata nell'unità appena assegnata. Se si usa il comando **popd** con le estensioni del comando abilitate, il comando **popd** rimuove l'assegnazione di lettera di unità creata da **pushd**.

## <a name="examples"></a>Esempi

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

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Popd](popd.md)