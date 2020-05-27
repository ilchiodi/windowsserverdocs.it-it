---
title: ombreggiatura elenco
description: Argomento di riferimento per il comando list shadows, che elenca le copie shadow persistenti e non persistenti presenti nel sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe568423-00ae-4ede-a1e7-07977cb50ad1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e0261a25c7a70a0c8690d578cadc9e73ff9a62e
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817171"
---
# <a name="list-shadows"></a>ombreggiatura elenco

Elenca le copie shadow persistenti e non persistenti presenti nel sistema.

## <a name="syntax"></a>Sintassi

```
list shadows {all | set <setID> | id <shadowID>}
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ---------- | ---------- |
| all | Elenca tutte le copie shadow. |
| set`<setID>` | Elenchi di ombreggiatura copie che appartengono all'ID specificato insieme di copie Shadow. |
| ID`<shadowID>` | Elenca qualsiasi copia shadow con l'ID di copia shadow specificata. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)