---
title: mput FTP
description: Argomento di riferimento per il comando ftp mput, che consente di copiare i file locali nel computer remoto usando il tipo di trasferimento di file corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5006c1ba19f0e017dea377b47bd0d89a68266382
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820411"
---
# <a name="ftp-mput"></a>mput FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia i file locali nel computer remoto utilizzando il tipo di trasferimento di file corrente.

## <a name="syntax"></a>Sintassi

```
mput <localfile>[ ]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<localfile>` | Specifica il file locale da copiare nel computer remoto. |

### <a name="examples"></a>Esempi

Per copiare *Program1. exe* e *Program2. exe* nel computer remoto usando il tipo di trasferimento file corrente, digitare:

```
mput Program1.exe Program2.exe
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando FTP ASCII](ftp-ascii.md)

- [comando binario FTP](ftp-binary.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
