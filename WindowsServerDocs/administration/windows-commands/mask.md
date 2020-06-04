---
title: mask
description: Argomento di riferimento per il comando mask, che consente di rimuovere le copie shadow dell'hardware importate tramite il comando Import.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ee01bb74b1fef1bb31a266c01a9e9bde7743691d
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354641"
---
# <a name="mask"></a>mask

Rimuove le copie shadow dell'hardware importate tramite il comando **Import** .

## <a name="syntax"></a>Sintassi

```
mask <shadowsetID>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| shadowsetID | Rimuove le copie shadow che appartengono all'ID del set di copie shadow specificato. |

#### <a name="remarks"></a>Commenti

- È possibile usare un alias esistente o una variabile di ambiente al posto di *ShadowSetID*. Utilizzare **aggiungere** senza parametri per visualizzare gli alias esistenti.

### <a name="examples"></a>Esempio

Per rimuovere la copia shadow importata *% Import_1%*, digitare:

```
mask %Import_1%
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)