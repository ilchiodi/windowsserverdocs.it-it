---
title: bitsadmin getbytestotal
description: 'Windows Commands Topic for **BITSAdmin getbytestotal** : recupera le dimensioni del processo specificato.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38b0f09e13919e0c75d2b7429dd66f765b434de5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381752"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal



Recupera le dimensioni del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetBytesTotal <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente vengono recuperate le dimensioni del processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetBytesTotal myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)