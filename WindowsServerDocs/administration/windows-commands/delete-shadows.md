---
title: Elimina ombre
description: Argomento di riferimento per il comando Elimina Shadows, che consente di eliminare le copie shadow.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b757314c96024741795c6770a98d10ac23b5bd0
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993105"
---
# <a name="delete-shadows"></a>Elimina ombre

Consente di eliminare le copie shadow.

## <a name="syntax"></a>Sintassi

```
delete shadows [all | volume <volume> | oldest <volume> | set <setID> | id <shadowID> | exposed {<drive> | <mountpoint>}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ---- | ---- |
| all | Elimina tutte le copie shadow. |
| volume`<volume>` | Elimina tutte le copie shadow del volume specificato. |
| meno recente`<volume>` | Elimina la copia shadow meno recente del volume specificato. |
| set`<setID>` | Elimina le copie shadow in un Set di copia Shadow dell'ID specificato. È possibile specificare un alias usando il **%** simbolo se l'alias esiste nell'ambiente corrente. |
| ID`<shadowID>` | Elimina una copia dell'ID specificato. È possibile specificare un alias usando il **%** simbolo se l'alias esiste nell'ambiente corrente. |
| esposto {'<drive> | <mountpoint>} |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Delete](delete.md)
