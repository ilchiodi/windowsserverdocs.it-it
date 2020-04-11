---
title: SetHttpMethod Bitsadmin
description: Windows Commands Topic for **BITSAdmin SetHttpMethod**, che imposta il verbo HTTP da usare.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: d349dcad7bdf6a6fc566ed961c3160836d7f49da
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122961"
---
# <a name="bitsadmin-sethttpmethod"></a>SetHttpMethod Bitsadmin

Imposta il verbo HTTP da usare.

## <a name="syntax"></a>Sintassi

```
bitsadmin /sethttpmethod <job> <httpmethod>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| lavoro | Nome visualizzato o GUID del processo. |
| HttpMethod | Verbo HTTP da usare. Per informazioni sui verbi disponibili, vedere [definizioni dei metodi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). |

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)