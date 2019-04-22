---
title: move
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d651e586c31ff64664079bdd10ffde3701ec317d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824992"
---
# <a name="move"></a>move



Sposta uno o più file da una directory in un'altra directory.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
move [{/y | /-y}] [<Source>] [<Target>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/y|Evita la visualizzazione verrà richiesto di confermare che si desidera sovrascrivere un file di destinazione esistente.|
|/-y|Cause verrà richiesto di confermare che si desidera sovrascrivere un file di destinazione esistente.|
|\<origine >|Specifica il percorso e il nome del file o file da spostare. Se si desidera spostare o rinominare una directory *origine* deve essere il percorso della directory corrente e il nome.|
|\<Target>|Specifica il percorso e il nome per spostare i file. Se si desidera spostare o rinominare una directory *destinazione* deve essere il percorso di directory desiderato e il nome.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Il **/y** opzione della riga di comando potrebbe essere preimpostato nella variabile di ambiente COPYCMD. È possibile eseguire l'override di questa operazione con **/-y** nella riga di comando. Il valore predefinito è per la richiesta prima di sovrascrivere i file, a meno che il **copia** comando viene eseguito dall'interno uno script batch.
-   Spostare i file crittografati in un volume che non supporta i risultati Encrypting File System (EFS) un errore. Decrittografare i file prima di tutto o spostare i file in un volume che supporta EFS.

## <a name="BKMK_examples"></a>Esempi

Per spostare tutti i file con estensione xls dalla directory \Data per le directory \Second_Q\Reports, digitare:
```
move \data\*.xls \second_q\reports\ 
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)