---
title: convert
description: Argomento di riferimento per il comando Convert, che converte un disco da un tipo di disco a un altro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ae151297-af21-4701-bd69-21d775518e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab7189ea774750f8de2ceaecd9511fc8c3a71a97
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720740"
---
# <a name="convert"></a>convert

Converte un disco da un tipo di disco a un altro.

## <a name="syntax"></a>Sintassi

```
convert basic
convert dynamic
convert gpt
convert mbr
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| [Converti comando di base](convert-basic.md) | Converte un disco dinamico vuoto in disco di base. |
| [Converti comando dinamico](convert-dynamic.md) | Converte un disco di base in un disco dinamico. |
| [Converti comando GPT](convert-gpt.md) | Converte un disco di base vuoto con lo stile di partizione MBR (master boot record) in un disco di base con lo stile di partizione GPT (tabella di partizione GUID). |
| [Converti comando MBR](convert-mbr.md) | Converte un disco di base vuoto con lo stile di partizione GPT (tabella di partizione GUID) in un disco di base con lo stile di partizione MBR (master boot record). |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
