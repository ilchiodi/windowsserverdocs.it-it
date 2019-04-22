---
title: MD
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1396038410ecc5db5a124a1768038c4f8c8bea8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820842"
---
# <a name="md"></a>MD



Crea una directory o sottodirectory.

> [!NOTE]
> Questo comando è analogo a come le **mkdir** comando.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
md [<Drive>:]<Path>
mkdir [<Drive>:]<Path>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Drive>:|Specifica l'unità in cui si desidera creare la nuova directory.|
|\<Path>|Obbligatorio. Specifica il nome e il percorso della nuova directory. La lunghezza massima di qualsiasi singolo percorso è determinata dal file system.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Le estensioni dei comandi, sono abilitati per impostazione predefinita, sarà possibile usare un unico **md** comando per creare directory intermedie in un percorso specificato.

## <a name="BKMK_examples"></a>Esempi

Per creare una directory denominata Directory1 all'interno della directory corrente, digitare:
```
md Directory1
```
Per creare l'albero di directory Taxes\Property\Current all'interno della directory radice, con le estensioni abilitate, digitare:
```
md \Taxes\Property\Current
```
Per creare l'albero di directory Taxes\Property\Current all'interno della directory radice dell'esempio precedente, ma con le estensioni dei comandi disabilitate, digitare la sequenza di comandi seguente:
```
md \Taxes
cd \Taxes 
md Property
cd Property
md Current
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

[Cmd](cmd.md)