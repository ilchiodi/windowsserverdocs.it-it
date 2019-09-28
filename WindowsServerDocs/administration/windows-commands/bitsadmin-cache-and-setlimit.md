---
title: cache Bitsadmin e limite
description: 'Argomento dei comandi di Windows per la **cache Bitsadmin e** la limitazione: imposta il limite delle dimensioni della cache.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88a10ce8599202e237daa6822cf62806d3c21429
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381943"
---
# <a name="bitsadmin-cache-and-setlimit"></a>cache Bitsadmin e limite



Imposta il limite delle dimensioni della cache.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Cache /SetLimit Percent
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Percent|Limite della cache definito come percentuale dello spazio totale su disco rigido.|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente la dimensione della cache viene limitata al 50%.
```
C:\>bitsadmin /Cache /SetLimit 50 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)