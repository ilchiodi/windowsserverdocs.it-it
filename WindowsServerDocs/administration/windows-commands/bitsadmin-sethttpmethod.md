---
title: bitsadmin sethttpmethod
description: Argomento di riferimento per il comando Bitsadmin SetHttpMethod, che imposta il verbo HTTP da usare.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: daf1c23565bc4f398fd29e51aaaeef23b3b0d018
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719353"
---
# <a name="bitsadmin-sethttpmethod"></a>bitsadmin sethttpmethod

Imposta il verbo HTTP da usare.

## <a name="syntax"></a>Sintassi

```
bitsadmin /sethttpmethod <job> <httpmethod>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |
| HttpMethod | Verbo HTTP da usare. Per informazioni sui verbi disponibili, vedere [definizioni dei metodi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
