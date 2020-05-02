---
title: add
description: Argomento di riferimento per il comando Aggiungi, che aggiunge volumi al set di volumi che devono essere copiati come Shadow, oppure aggiunge alias all'ambiente alias.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b621a3061c4e3366085c5cc44f91f26dd33d4e3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719010"
---
# <a name="add"></a>add

Aggiunge volumi al set di volumi di cui deve essere eseguita la copia shadow o aggiunge alias all'ambiente alias. Se usato senza sottocomandi, **Aggiungi** elenca i volumi e gli alias correnti.

> [!NOTE]
> Gli alias non vengono aggiunti all'ambiente alias fino a quando non viene creata la copia shadow. Gli alias che è necessario aggiungere immediatamente devono essere aggiunti usando **Aggiungi alias**.

## <a name="syntax"></a>Sintassi

```
add
add volume <volume> [provider <providerid>]
add alias <aliasname> <aliasvalue>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ---------- | ----------- |
| volume | Aggiunge un volume al set di copie shadow, ovvero il set di volumi da replicare. Vedere [aggiungere un volume](add-volume.md) per la sintassi e i parametri. |
| alias | Aggiunge il nome e il valore specificati all'ambiente alias. Vedere [aggiungere alias](add-alias.md) per sintassi e parametri. |
| /? | Visualizza la guida nella riga di comando. |

## <a name="examples"></a>Esempi

Per visualizzare i volumi aggiunti e gli alias attualmente presenti nell'ambiente, digitare:

```
add
```

L'output seguente mostra che l'unità C è stata aggiunta al set di copie shadow:

```
Volume c: alias System1    GUID \\?\Volume{XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}\
1 volume in Shadow Copy Set.
No Diskshadow aliases in the environment.
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)