---
title: move
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: efd0cd0716c564a9570647820056ab9c38e41274
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839374"
---
# <a name="move"></a>move



Sposta uno o più file da una directory a un'altra directory.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
move [{/y | /-y}] [<Source>] [<Target>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/y|Evita la richiesta di conferma della sovrascrittura di un file di destinazione esistente.|
|/-y|Fa in modo che la richiesta di conferma che si desidera sovrascrivere un file di destinazione esistente.|
|> origine \<|Specifica il percorso e il nome del file o dei file da spostare. Se si vuole spostare o rinominare una directory, l' *origine* deve essere il percorso e il nome della directory corrente.|
|> di destinazione \<|Specifica il percorso e il nome in cui spostare i file. Se si vuole spostare o rinominare una directory, la *destinazione* deve essere il percorso e il nome della directory desiderata.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   È possibile impostare l'opzione della riga di comando **/y** nella variabile di ambiente COPYCMD. È possibile eseguire l'override di questa operazione con **/-y** nella riga di comando. Per impostazione predefinita, viene richiesto di richiedere prima di sovrascrivere i file, a meno che il comando **Copy** non venga eseguito dall'interno di uno script batch.
-   Lo trasferimento di file crittografati in un volume che non supporta Encrypting File System (EFS) genera un errore. Decrittografare i file prima o spostare i file in un volume che supporta EFS.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per spostare tutti i file con estensione xls dalla directory \Data alla directory \ Second_Q \Rapporti, digitare:
```
move \data\*.xls \second_q\reports\ 
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)