---
title: mklink
description: Argomento di riferimento per il comando mklink, che consente di creare una directory o un file con simboli o collegamenti reali.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f998533ce3184213786a341c2413e7323496e96
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354611"
---
# <a name="mklink"></a>mklink

Crea una directory o un file con collegamento simbolico o reale.

## <a name="syntax"></a>Sintassi

```
mklink [[/d] | [/h] | [/j]] <link> <target>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /d | Crea un collegamento simbolico di directory. Per impostazione predefinita, questo comando crea un collegamento simbolico file. |
| /h | Crea un collegamento reale anziché un collegamento simbolico. |
| /j | Crea una giunzione di directory. |
| `<link>` | Specifica il nome del collegamento simbolico da creare. |
| `<target>` | Specifica il percorso (relativo o assoluto) a cui fa riferimento il nuovo collegamento simbolico. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="examples"></a>Esempio

Per creare e rimuovere un collegamento simbolico denominato, MyFile e MyFile. file, dalla directory radice alla directory \Users\User1\Documents e un file example. file che si trova nella directory, digitare:

```
mklink /d \MyFolder \Users\User1\Documents
mklink /h \MyFile.file \User1\Documents\example.file
rd \MyFolder
del \MyFile.file
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [del comando](del.md)

- [comando Rd](rd.md)

- [New-Item in Windows PowerShell](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
