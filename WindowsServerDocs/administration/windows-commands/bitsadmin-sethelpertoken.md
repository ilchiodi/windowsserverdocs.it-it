---
title: bitsadmin sethelpertoken
description: Argomento di riferimento per il comando Bitsadmin sethelpertoken, che imposta il token primario del prompt dei comandi corrente (o un token di account utente locale arbitrario, se specificato) come token helper del processo di trasferimento BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: b125f95e262c2fd78f20266e3e2b6c80cea5a789
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719399"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

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
| processo | Nome visualizzato o GUID del processo. |
| `<username@domain>` `<password>` | Facoltativa. Le credenziali dell'account utente locale per il token da usare. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
