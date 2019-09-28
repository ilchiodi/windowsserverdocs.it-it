---
title: bitsadmin getcompletiontime
description: "Windows Commands Topic for **BITSAdmin getcompletiontime** : Recupera l'ora in cui il processo ha completato il trasferimento dei dati."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 190467d5f3a7b7244ed0d7ab3b75d4cbbf56c8d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381740"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime



Recupera l'ora in cui il processo ha completato il trasferimento dei dati.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetCompletionTime <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene recuperata l'ora in cui il processo denominato *myDownloadJob* ha terminato il trasferimento dei dati.
```
C:\>bitsadmin /GetCompletionTime myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)