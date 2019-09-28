---
title: Elimina ombre
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c965af8b045c5ab3a110542d148b255f382a95c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378633"
---
# <a name="delete-shadows"></a>Elimina ombre



Consente di eliminare le copie shadow.

## <a name="syntax"></a>Sintassi

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

## <a name="parameters"></a>Parametri

|     Parametro     |                                                                             Descrizione                                                                              |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        tutti        |                                                                      Elimina tutte le copie shadow.                                                                      |
| volume \<Volume >  |                                                            Elimina tutte le copie shadow del volume specificato.                                                            |
| \<Volume meno recenti >  |                                                         Elimina la copia shadow meno recente del volume specificato.                                                          |
|   imposta \<SetID >    | Elimina le copie shadow in un Set di copia Shadow dell'ID specificato. È possibile specificare un alias utilizzando il **%** simbolo, se l'alias è presente nell'ambiente corrente. |
|  ID \<ShadowID >   |              Elimina una copia dell'ID specificato. È possibile specificare un alias utilizzando il **%** simbolo, se l'alias è presente nell'ambiente corrente.               |
| esposto {\<Drive > |                                                                            <MountPoint>}                                                                             |

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)