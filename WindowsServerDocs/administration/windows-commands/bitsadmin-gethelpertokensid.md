---
title: bitsadmin gethelpertokensid
description: Argomento di riferimento per il comando Bitsadmin gethelpertokensid, che restituisce il SID di un token helper del processo di trasferimento BITS, se ne è stato impostato uno.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: c45bf86d8a7364289db41fa390f319270a2a8386
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717899"
---
# <a name="bitsadmin-gethelpertokensid"></a>bitsadmin gethelpertokensid

Restituisce il SID di un [token di supporto](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)del processo di trasferimento BITS, se ne è stato impostato uno.

> [!NOTE]
> Questo comando non è supportato da BITS 3,0 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /gethelpertokensid <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare il SID di un processo di trasferimento BITS denominato *myDownloadJob*:

```
bitsadmin /gethelpertokensid myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
