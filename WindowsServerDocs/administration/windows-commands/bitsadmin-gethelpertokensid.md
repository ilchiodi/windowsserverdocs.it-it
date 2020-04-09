---
title: gethelpertokensid Bitsadmin
description: Windows Commands Topic for **BITSAdmin gethelpertokensid**, che restituisce il SID di un token di supporto del processo di trasferimento BITS, se ne è stato impostato uno.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: a2e26ff459b068595529fbd24e6165c130660570
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850644"
---
# <a name="bitsadmin-gethelpertokensid"></a>gethelpertokensid Bitsadmin

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
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
