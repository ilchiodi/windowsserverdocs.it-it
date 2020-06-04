---
title: mkdir
description: Argomento di riferimento per il comando mkdir, che consente di creare una directory o una sottodirectory.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 033a57a2-5deb-4c98-aa78-61ce8df2a330
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 921e369cb1550bca8e26cc0beada4c1f5c1c3d5b
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354601"
---
# <a name="mkdir"></a>mkdir

Crea una directory o una sottodirectory. Le estensioni dei comandi, abilitate per impostazione predefinita, consentono di usare un singolo comando **mkdir** per creare directory intermedie in un percorso specificato.

> [!NOTE]
> Questo comando è lo stesso del [comando MD](md.md).

## <a name="syntax"></a>Sintassi

```
mkdir [<drive>:]<path>
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
mkdir Directory1
```

Per creare l'albero di directory *Taxes\Property\Current* all'interno della directory radice, con le estensioni del comando abilitate, digitare:

```
mkdir \Taxes\Property\Current
```

Per creare l'albero di directory *Taxes\Property\Current* all'interno della directory radice come nell'esempio precedente, ma con le estensioni del comando disabilitate, digitare la sequenza di comandi seguente:

```
mkdir \Taxes
mkdir \Taxes\Property
mkdir \Taxes\Property\Current
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando MD](md.md)