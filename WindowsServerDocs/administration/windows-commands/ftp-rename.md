---
title: ridenominazione FTP
description: Argomento di riferimento per il comando di ridenominazione FTP, che rinomina i file remoti.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8d3ea25e48266db6a4a282f2ea395bd8b8d5fd9
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820311"
---
# <a name="ftp-rename"></a>ridenominazione FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rinomina i file remoti.

## <a name="syntax"></a>Sintassi

```
rename <filename> <newfilename>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<filename>` | Specifica il file che si desidera rinominare. |
| `<newfilename>` | Specifica il nuovo nome del file. |

### <a name="examples"></a>Esempi

Per rinominare il file remoto *example. txt* in *Example1. txt*, digitare:

```
rename example.txt example1.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
