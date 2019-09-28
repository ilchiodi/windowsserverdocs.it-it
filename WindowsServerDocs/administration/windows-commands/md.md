---
title: MD
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
ms.openlocfilehash: 965a5c506535a2c52d6cc7b3557c6104182c12a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373692"
---
# <a name="md"></a>MD



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
|> \<Drive:|Consente di specificare l'unità in cui si desidera creare la nuova directory.|
|\<Path >|Obbligatorio. Specifica il nome e il percorso della nuova directory. La lunghezza massima di un singolo percorso è determinata dalla file system.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Le estensioni dei comandi, abilitate per impostazione predefinita, consentono di usare un singolo comando **MD** per creare directory intermedie in un percorso specificato.

## <a name="BKMK_examples"></a>Esempi

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
cd \Taxes 
md Property
cd Property
md Current
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Cmd](cmd.md)