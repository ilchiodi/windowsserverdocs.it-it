---
title: mdelete FTP
description: Argomento di riferimento per il comando ftp mdelete, che consente di eliminare i file nel computer remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8ee1882878ce06a16bd6ff6f0dcaa512d6d8b56a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820231"
---
# <a name="ftp-mdelete"></a>mdelete FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina i file nel computer remoto.

## <a name="syntax"></a>Sintassi
```
mdelete <remotefile>[...]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<remotefile>` | Specifica il file remoto da eliminare. |

### <a name="examples"></a>Esempi

Per eliminare i file remoti *a. exe* e *b. exe*, digitare:

```
mdelete a.exe b.exe
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
