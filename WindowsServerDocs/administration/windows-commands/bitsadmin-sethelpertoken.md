---
title: sethelpertoken Bitsadmin
description: Windows Commands Topic for **BITSAdmin sethelpertoken**, che imposta il token primario del prompt dei comandi corrente (o un token dell'account utente locale arbitrario, se specificato) come token helper del processo di trasferimento BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: ba4b9a4ed1b59d1b1aeda30353317739b7fdfa9e
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122986"
---
# <a name="bitsadmin-sethelpertoken"></a>sethelpertoken Bitsadmin

Imposta il token primario del prompt dei comandi corrente (o un token dell'account utente locale arbitrario, se specificato) come [token Helper](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)del processo di trasferimento BITS.

> [!NOTE]
> Questo comando non è supportato da BITS 3,0 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /sethelpertoken <job> [<user_name@domain> <password>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| lavoro | Nome visualizzato o GUID del processo. |
| `<username@domain>` `<password>` | Facoltativa. Le credenziali dell'account utente locale per il token da usare. |

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
