---
title: mask
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 816bcd932091b33ed897add5a13603e3a1eea925
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724014"
---
# <a name="mask"></a>mask



Rimuove le copie shadow dell'hardware importate tramite il comando **Import** .



## <a name="syntax"></a>Sintassi

```
mask <ShadowSetID>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|ShadowSetID|Rimuove le copie shadow che appartengono all'ID del set di copie shadow specificato.|

## <a name="remarks"></a>Osservazioni

-   È possibile usare un alias esistente o una variabile di ambiente al posto di *ShadowSetID*. Utilizzare **aggiungere** senza parametri per visualizzare gli alias esistenti.

## <a name="examples"></a>Esempi

Per rimuovere la copia shadow importata% Import_1%, digitare:
```
mask %Import_1%
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)