---
title: Md
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3751b185677bfee9d0519b9a617bea1df063c1e7
ms.sourcegitcommit: 51e0b575ef43cd16b2dab2db31c1d416e66eebe8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76259076"
---
# <a name="md"></a>Md



Crea una directory o una sottodirectory.

> [!NOTE]
> Questo comando è lo stesso del comando **mkdir** .

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
md [<Drive>:]<Path>
mkdir [<Drive>:]<Path>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|> unità \<:|Consente di specificare l'unità in cui si desidera creare la nuova directory.|
|Percorso \<|Obbligatorio. Specifica il nome e il percorso della nuova directory. La lunghezza massima di un singolo percorso è determinata dalla file system.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

Le estensioni dei comandi, abilitate per impostazione predefinita, consentono di usare un singolo comando **MD** per creare directory intermedie in un percorso specificato.

## <a name="BKMK_examples"></a>Esempi:

Per creare una directory denominata directory1 nella directory corrente, digitare:
```
md Directory1
```
Per creare l'albero di directory Taxes\Property\Current all'interno della directory radice, con le estensioni del comando abilitate, digitare:
```
md \Taxes\Property\Current
```
Per creare l'albero di directory Taxes\Property\Current all'interno della directory radice come nell'esempio precedente, ma con le estensioni del comando disabilitate, digitare la sequenza di comandi seguente:
```
md \Taxes
md \Taxes\Property
md \Taxes\Property\Current
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Cmd](cmd.md)