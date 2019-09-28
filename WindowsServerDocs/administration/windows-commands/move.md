---
title: move
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4302a3403f73892500c3f9deb608e9489c7e91ae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373535"
---
# <a name="move"></a>move



Sposta uno o più file da una directory a un'altra directory.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
move [{/y | /-y}] [<Source>] [<Target>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/y|Evita la richiesta di conferma della sovrascrittura di un file di destinazione esistente.|
|/-y|Fa in modo che la richiesta di conferma che si desidera sovrascrivere un file di destinazione esistente.|
|\<Source >|Specifica il percorso e il nome del file o dei file da spostare. Se si vuole spostare o rinominare una directory, l' *origine* deve essere il percorso e il nome della directory corrente.|
|\<Target >|Specifica il percorso e il nome in cui spostare i file. Se si vuole spostare o rinominare una directory, la *destinazione* deve essere il percorso e il nome della directory desiderata.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   È possibile impostare l'opzione della riga di comando **/y** nella variabile di ambiente COPYCMD. È possibile eseguire l'override di questa operazione con **/-y** nella riga di comando. Per impostazione predefinita, viene richiesto di richiedere prima di sovrascrivere i file, a meno che il comando **Copy** non venga eseguito dall'interno di uno script batch.
-   Lo trasferimento di file crittografati in un volume che non supporta Encrypting File System (EFS) genera un errore. Decrittografare i file prima o spostare i file in un volume che supporta EFS.

## <a name="BKMK_examples"></a>Esempi

Per spostare tutti i file con estensione xls dalla directory \Data alla directory \Second_Q\Reports, digitare:
```
move \data\*.xls \second_q\reports\ 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)