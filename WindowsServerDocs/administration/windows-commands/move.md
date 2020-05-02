---
title: spostamento
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1df37753239fea5d5ba9ba22256706a47d4c6a2f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723918"
---
# <a name="move"></a>spostamento



Sposta uno o più file da una directory a un'altra directory.



## <a name="syntax"></a>Sintassi

```
move [{/y | /-y}] [<Source>] [<Target>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/y|Evita la richiesta di conferma della sovrascrittura di un file di destinazione esistente.|
|/-y|Fa in modo che la richiesta di conferma che si desidera sovrascrivere un file di destinazione esistente.|
|\<> di origine|Specifica il percorso e il nome del file o dei file da spostare. Se si vuole spostare o rinominare una directory, l' *origine* deve essere il percorso e il nome della directory corrente.|
|\<Target>|Specifica il percorso e il nome in cui spostare i file. Se si vuole spostare o rinominare una directory, la *destinazione* deve essere il percorso e il nome della directory desiderata.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   È possibile impostare l'opzione della riga di comando **/y** nella variabile di ambiente COPYCMD. È possibile eseguire l'override di questa operazione con **/-y** nella riga di comando. Per impostazione predefinita, viene richiesto di richiedere prima di sovrascrivere i file, a meno che il comando **Copy** non venga eseguito dall'interno di uno script batch.
-   Lo trasferimento di file crittografati in un volume che non supporta Encrypting File System (EFS) genera un errore. Decrittografare i file prima o spostare i file in un volume che supporta EFS.

## <a name="examples"></a>Esempi

Per spostare tutti i file con estensione xls dalla directory \Data alla directory \ Second_Q \Rapporti, digitare:
```
move \data\*.xls \second_q\reports\ 
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)