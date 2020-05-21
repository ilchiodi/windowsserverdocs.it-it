---
title: exec
description: Argomento di riferimento per il comando exec, che esegue un file di script nel computer locale.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 956f3d4a7c5992980aea0fc0f5933ee7def48381
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436107"
---
# <a name="exec"></a>exec

Esegue un file di script nel computer locale. Questo comando Duplica o Ripristina i dati come parte di una sequenza di backup o ripristino. Se lo script non riesce, viene restituito un errore e viene chiuso DiskShadow.

Il file può essere un **cmd** script.

## <a name="syntax"></a>Sintassi

```
exec <scriptfile.cmd>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<scriptfile.cmd>` | Specifica il file di script da eseguire. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando DiskShadow](diskshadow.md)
