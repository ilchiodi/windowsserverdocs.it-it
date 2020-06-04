---
title: md
description: Argomento di riferimento per il comando MD, che consente di creare una directory o una sottodirectory.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 928454c406216547783921005c9ff036a2844686
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354636"
---
# <a name="md"></a>md

Crea una directory o una sottodirectory. Le estensioni dei comandi, abilitate per impostazione predefinita, consentono di usare un singolo comando **MD** per creare directory intermedie in un percorso specificato.

> [!NOTE]
> Questo comando è lo stesso del [comando mkdir](mkdir.md).

## <a name="syntax"></a>Sintassi

```
md [<drive>:]<path>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<drive>`: | Consente di specificare l'unità in cui si desidera creare la nuova directory. |
| `<path>` | Specifica il nome e il percorso della nuova directory. La lunghezza massima di un singolo percorso è determinata dalla file system. Parametro obbligatorio. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="examples"></a>Esempio

Per creare una directory denominata *directory1* nella directory corrente, digitare:

```
md Directory1
```

Per creare l'albero di directory *Taxes\Property\Current* all'interno della directory radice, con le estensioni del comando abilitate, digitare:

```
md \Taxes\Property\Current
```

Per creare l'albero di directory *Taxes\Property\Current* all'interno della directory radice come nell'esempio precedente, ma con le estensioni del comando disabilitate, digitare la sequenza di comandi seguente:

```
md \Taxes
md \Taxes\Property
md \Taxes\Property\Current
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando mkdir](mkdir.md)