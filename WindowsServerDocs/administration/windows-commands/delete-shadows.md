---
title: Elimina ombre
description: Argomento di riferimento per le ombreggiature Delete, che elimina le copie shadow.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1dd367d76ad1699321af9caf47a0ddc351088a05
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720802"
---
# <a name="delete-shadows"></a>Elimina ombre

Consente di eliminare le copie shadow.

## <a name="syntax"></a>Sintassi

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ---- | ---- |
| all | Elimina tutte le copie shadow. |
| \<volume volume> | Elimina tutte le copie shadow del volume specificato. |
| > \<del volume meno recente | Elimina la copia shadow meno recente del volume specificato. |
| imposta \<> SetID | Elimina le copie shadow in un Set di copia Shadow dell'ID specificato. È possibile specificare un alias usando il **%** simbolo se l'alias esiste nell'ambiente corrente. |
| ID \<IDShadow> | Elimina una copia dell'ID specificato. È possibile specificare un alias usando il **%** simbolo se l'alias esiste nell'ambiente corrente. |
| esposte {\<unità> | <MountPoint>} |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)