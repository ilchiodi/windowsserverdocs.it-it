---
title: Elimina ombre
description: Windows Commands argomento for delete Shadows, che elimina le copie shadow.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd109f7ddc0365d03737eddba31a1a4b7f34915b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846564"
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
| tutti | Elimina tutte le copie shadow. |
| volume \<volume > | Elimina tutte le copie shadow del volume specificato. |
| Volume \<meno recente > | Elimina la copia shadow meno recente del volume specificato. |
| imposta \<SetId > | Elimina le copie shadow in un Set di copia Shadow dell'ID specificato. È possibile specificare un alias utilizzando il **%** simbolo, se l'alias è presente nell'ambiente corrente. |
| ID \<IDShadow > | Elimina una copia dell'ID specificato. È possibile specificare un alias utilizzando il **%** simbolo, se l'alias è presente nell'ambiente corrente. |
| esposte {\<unità > | <MountPoint>} |

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)