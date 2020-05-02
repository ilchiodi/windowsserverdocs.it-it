---
title: destinazione BdeHdCfg
description: Argomento di riferimento per il comando BdeHdCfg target, che prepara una partizione da utilizzare come unità di sistema da BitLocker e Windows Recovery.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f761d25d-8349-4ac7-ac46-6bb340a4348f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7f98f42675a49ab34ca1cf759efb9d40a69c38a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718592"
---
# <a name="bdehdcfg-target"></a>BdeHdCfg: destinazione

Prepara una partizione per l'utilizzo come unità di sistema da BitLocker e ripristino di Windows. Per impostazione predefinita, questa partizione viene creata senza una lettera di unità.

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge}
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| default | Indica che lo strumento da riga di comando seguirà lo stesso processo della configurazione guidata BitLocker. |
| unallocated | Crea la partizione di sistema dallo spazio non allocato disponibile sul disco. |
| `<drive_letter>`strizzacervelli | Riduce l'unità specificata della quantità necessaria per creare una partizione di sistema attiva. Per utilizzare questo comando, sull'unità specificata deve essere presente almeno il 5% di spazio disponibile. |
| `<drive_letter>`merge | Utilizza l'unità specificata come partizione di sistema attiva. L'unità del sistema operativo non può essere utilizzata come destinazione per l'unione. |

## <a name="examples"></a>Esempi

Per designare un'unità esistente (P) come unità di sistema:

```
bdehdcfg -target P: merge
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
