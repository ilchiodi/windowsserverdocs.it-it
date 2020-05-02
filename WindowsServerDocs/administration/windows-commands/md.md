---
title: MD
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a605571fb74af99d0f365a100dd33fd4db0d3f22
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724000"
---
# <a name="md"></a>MD



Crea una directory o una sottodirectory.

> [!NOTE]
> Questo comando è lo stesso del comando **mkdir** .



## <a name="syntax"></a>Sintassi

```
md [<Drive>:]<Path>
mkdir [<Drive>:]<Path>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> unità:|Consente di specificare l'unità in cui si desidera creare la nuova directory.|
|\<> percorso|Obbligatorio. Specifica il nome e il percorso della nuova directory. La lunghezza massima di un singolo percorso è determinata dalla file system.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

Le estensioni dei comandi, abilitate per impostazione predefinita, consentono di usare un singolo comando **MD** per creare directory intermedie in un percorso specificato.

## <a name="examples"></a>Esempi

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Cmd](cmd.md)