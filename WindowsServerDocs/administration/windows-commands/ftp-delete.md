---
title: eliminazione FTP
description: Argomento di riferimento per il comando FTP Delete, che consente di eliminare i file nei computer remoti.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 067c45f3-e4e8-4450-b8b6-836994f6adfe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43ea2ef90f29970a42b0196717ce4d2d3552a32c
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819952"
---
# <a name="ftp-delete"></a>eliminazione FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina i file nei computer remoti.

## <a name="syntax"></a>Sintassi

```
delete <remotefile>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<remotefile>` | Specifica il file da eliminare. |

### <a name="examples"></a>Esempi

Per eliminare il file *test. txt* nel computer remoto, digitare:

```
delete test.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
