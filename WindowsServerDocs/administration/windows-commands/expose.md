---
title: esporre
description: Argomento di riferimento per il comando Expose, che espone una copia shadow permanente come una lettera di unità, una condivisione o un punto di montaggio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d4e8ebf71f6ddcb457460f8174793586e81c73a6
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437176"
---
# <a name="expose"></a>esporre

Espone una copia permanente come una lettera di unità, una condivisione o un punto di montaggio.

## <a name="syntax"></a>Sintassi

```
expose <shadowID> {<drive:> | <share> | <mountpoint>}
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| IDShadow | Specifica l'ID di ombreggiatura della copia shadow che si desidera esporre. È anche possibile usare un alias esistente o una variabile di ambiente al posto di *IDShadow*. Utilizzare **aggiungere** senza parametri per visualizzare gli alias esistenti. |
| `<drive:>` | Espone la copia shadow specificata come lettera di unità (ad esempio, `p:` ). |
| `<share>` | Espone la copia shadow specificata in una condivisione, ad esempio `\\machinename` .   |
| `<mountpoint>` | Espone la copia shadow specificata a un punto di montaggio (ad esempio, `C:\shadowcopy` ). |

### <a name="examples"></a>Esempi

Per esporre la copia shadow persistente associata alla variabile di ambiente VSS_SHADOW_1 come unità X, digitare:

```
expose %vss_shadow_1% x:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando DiskShadow](diskshadow.md)
