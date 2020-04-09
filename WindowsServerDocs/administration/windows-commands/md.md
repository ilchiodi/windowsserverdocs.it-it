---
title: MD
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2fad89fe4b7e8425064301f6020fefaa5705b25
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839584"
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

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|> unità \<:|Consente di specificare l'unità in cui si desidera creare la nuova directory.|
|Percorso \<>|Obbligatoria. Specifica il nome e il percorso della nuova directory. La lunghezza massima di un singolo percorso è determinata dalla file system.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Le estensioni dei comandi, abilitate per impostazione predefinita, consentono di usare un singolo comando **MD** per creare directory intermedie in un percorso specificato.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Cmd](cmd.md)