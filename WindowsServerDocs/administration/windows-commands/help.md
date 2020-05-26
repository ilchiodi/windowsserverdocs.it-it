---
title: help
description: Argomento di riferimento per il comando della guida, che visualizza un elenco dei comandi disponibili o informazioni dettagliate della guida su un comando specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c65b5ac3-711a-4368-95b8-ba82e2d00713
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b73ef32b49b834a91f24e943749eb21398c8588
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818661"
---
# <a name="help"></a>help

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza un elenco di comandi disponibili o le informazioni della Guida dettagliate su un comando specificato. Se utilizzata senza parametri, **Guida** vengono elencate e descritte brevemente ogni comando di sistema.

## <a name="syntax"></a>Sintassi

```
help [<command>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<command>` | Specifica il comando per cui visualizzare informazioni dettagliate della Guida. |

### <a name="examples"></a>Esempi

Per visualizzare informazioni sul comando **Robocopy** , digitare:

```
help robocopy
```

Per visualizzare un elenco di tutti i comandi disponibili in DiskPart, digitare:

```
help
```

Per visualizzare le informazioni della Guida dettagliate su come utilizzare il **Crea partizione primaria** comando DiskPart, tipo:

```
help create partition primary
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
