---
title: mget FTP
description: Argomento di riferimento per il comando FTP mget, che consente di copiare i file remoti nel computer locale utilizzando il tipo di trasferimento di file corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 93d562fcdaec0d6609e7be8b3543cd55bb910238
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820431"
---
# <a name="ftp-mget"></a>mget FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia i file remoti nel computer locale utilizzando il tipo di trasferimento di file corrente.

## <a name="syntax"></a>Sintassi

```
mget <remotefile>[ ]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<remotefile>` | Specifica i file remoti da copiare nel computer locale. |

### <a name="examples"></a>Esempi

Per copiare i file remoti a *. exe* e *b. exe* nel computer locale usando il tipo di trasferimento file corrente, digitare:

```
mget a.exe b.exe
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando FTP ASCII](ftp-ascii.md)

- [comando binario FTP](ftp-binary.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
