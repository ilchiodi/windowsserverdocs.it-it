---
title: setmaxdownloadtime Bitsadmin
description: Argomento dei comandi di Windows per **BITSAdmin setmaxdownloadtime** -imposta il timeout di download in secondi.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 985453de5bd2f4a06b5635ae5b0a9794d30175b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380564"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>setmaxdownloadtime Bitsadmin



Imposta il timeout del download in secondi.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetMaxDownloadTime <Job> <Timeout>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Timeout|Il timeout in secondi|

## <a name="remarks"></a>Note

-   N/D

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente imposta il timeout per il processo denominato *myDownloadJob* su 10 secondi.
```
C:\>bitsadmin /SetMaxDownloadTime myDownloadJob 10
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)