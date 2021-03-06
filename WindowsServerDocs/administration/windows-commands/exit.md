---
title: exit
description: Argomento di riferimento per Exit, che esce dall'interprete dei comandi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d3cee4a2-6210-46f0-b8e4-7381c3c4e530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdfec9861e63f7484a9c45c45a22d19873cabbe9
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819501"
---
# <a name="exit"></a>exit

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esce dall'interprete dei comandi o dallo script batch corrente.

## <a name="syntax"></a>Sintassi

```
exit [/b] [<exitcode>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| / b | Chiude lo script batch corrente anziché uscire Cmd.exe. Se eseguita in uno script batch, viene chiuso Cmd.exe. |
| `<exitcode>` | Specifica un numero. Se **/b** è specificato, la variabile di ambiente ERRORLEVEL è impostata su tale numero. Se si sta uscendo dall'interprete dei comandi, il codice di uscita del processo viene impostato su tale numero. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per chiudere l'interprete dei comandi, digitare:

```
exit
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
