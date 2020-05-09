---
title: diskperf
description: Argomento di riferimento per il comando diskperf, che può essere utilizzato per abilitare o disabilitare in remoto i contatori delle prestazioni dei dischi fisici o logici nei computer che eseguono Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 092518f414d6e27436c46ffd6f9f15b6e6c0407e
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992431"
---
# <a name="diskperf"></a>diskperf

Il comando **diskperf** Abilita o Disabilita in remoto i contatori delle prestazioni dei dischi fisici o logici nei computer che eseguono Windows.

## <a name="syntax"></a>Sintassi

```
diskperf [-y[d|v] | -n[d|v]] [\\computername]
```

## <a name="options"></a>Opzioni

| Opzione | Descrizione |
| ------ | ----------- |
| -y | Avvia tutti i contatori delle prestazioni del disco quando il computer viene riavviato. |
| -iarde | Abilita i contatori delle prestazioni del disco per le unità fisiche quando il computer viene riavviato. |
| -YV | Abilita i contatori delle prestazioni del disco per le unità logiche o i volumi di archiviazione quando il computer viene riavviato. |
| -n | Disabilita tutti i contatori delle prestazioni del disco quando il computer viene riavviato. |
| -ND | Disabilitare i contatori delle prestazioni del disco per le unità fisiche quando il computer viene riavviato. |
| -NV | Disabilitare i contatori delle prestazioni del disco per le unità logiche o i volumi di archiviazione al riavvio del computer. |
| `\\<computername>` | Specifica il nome del computer in cui si desidera abilitare o disabilitare i contatori delle prestazioni del disco. |
| -? | Vengono visualizzate sensibile al contesto della Guida. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
