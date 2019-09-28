---
title: Maschera
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f0dc83d7d9f7204f56e95c62b7cfad991f539ef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373706"
---
# <a name="mask"></a>Maschera



rimuove le copie shadow dell'hardware importate tramite il comando **Import** .

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
mask <ShadowSetID>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|ShadowSetID|Rimuove le copie shadow che appartengono all'ID del set di copie shadow specificato.|

## <a name="remarks"></a>Note

-   È possibile usare un alias esistente o una variabile di ambiente al posto di *ShadowSetID*. Utilizzare **aggiungere** senza parametri per visualizzare gli alias esistenti.

## <a name="BKMK_examples"></a>Esempi

Per rimuovere la copia shadow importata% Import_1%, digitare:
```
mask %Import_1%
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)