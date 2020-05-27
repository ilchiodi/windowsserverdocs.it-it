---
title: utente FTP
description: Argomento di riferimento per il comando FTP User, che specifica un utente per il computer remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0773084ee718db37d6c79009d66d754283f94c8
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820261"
---
# <a name="ftp-user"></a>utente FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Specifica un utente al computer remoto.

## <a name="syntax"></a>Sintassi

```
user <username> [<password>] [<account>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<username>` | Specifica un nome utente con cui si accede al computer remoto. |
| `[<password>]` | Specifica la password per il *nome utente*. Se non si specifica una password, ma è obbligatoria, il comando **FTP** richiede la password. |
| `[<account>]` | Specifica un account con cui si accede al computer remoto. Se un *account* non è specificato, ma è obbligatorio, il comando **FTP** richiede l'account. |

### <a name="examples"></a>Esempi

Per specificare *User1* con la password *Password1*, digitare:

```
user User1 Password1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
