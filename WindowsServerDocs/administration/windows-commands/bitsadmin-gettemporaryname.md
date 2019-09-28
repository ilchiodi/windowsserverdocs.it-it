---
title: gettemporaryname Bitsadmin
description: "Argomento dei comandi di Windows per **BITSAdmin gettemporaryname** : indica il nome di file temporaneo del file specificato all'interno del processo."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b665fae4c0bfdd5ea04b929be49f9590430b358
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381297"
---
# <a name="bitsadmin-gettemporaryname"></a>gettemporaryname Bitsadmin



Restituisce il nome del file temporaneo del file specificato all'interno del processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetTemporaryName <Job> <file index> 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Indice file|Inizia da 0|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente restituisce il nome del file temporaneo del file 2 per il processo denominato *myJob*.
```
C:\>bitsadmin /GetTemporaryName myJob 1 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)