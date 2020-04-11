---
title: bitsadmin takeownership
description: Windows Commands Topic for **BITSAdmin TakeOwnership**, che consente a un utente con privilegi amministrativi di assumere la proprietà del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a04f54747e3e06aa61166c2c9f9cedfdfbc8d42a
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122690"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership

Consente a un utente con privilegi amministrativi di assumere la proprietà del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /takeownership <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ---------- |
| Job | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

L'esempio seguente acquisisce la proprietà del processo denominato *myDownloadJob*.

```
C:\>bitsadmin /takeownership myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)