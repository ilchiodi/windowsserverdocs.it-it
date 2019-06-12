---
title: eliminare le ombreggiature
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1a0945477bc4fce907b5ec4a697c7a2ec2f59557
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436113"
---
# <a name="delete-shadows"></a>eliminare le ombreggiature



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
| meno recente \<Volume >  |                                                         Elimina la copia shadow meno recente del volume specificato.                                                          |
|   set \<SetID>    | Elimina le copie shadow in un Set di copia Shadow dell'ID specificato. È possibile specificare un alias utilizzando il **%** simbolo, se l'alias è presente nell'ambiente corrente. |
|  id \<ShadowID>   |              Elimina una copia dell'ID specificato. È possibile specificare un alias utilizzando il **%** simbolo, se l'alias è presente nell'ambiente corrente.               |
| esposti {\<Drive > |                                                                            <MountPoint>}                                                                             |

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)