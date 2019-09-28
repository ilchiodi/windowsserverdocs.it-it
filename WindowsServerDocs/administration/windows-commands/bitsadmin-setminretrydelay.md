---
title: bitsadmin setminretrydelay
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 379dfa8bfdc48969f268fd1c9544d3bee8bbe646
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380508"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

Imposta il periodo di tempo minimo, espresso in secondi, di attesa dei bit dopo che si è verificato un errore temporaneo prima di provare a trasferire il file.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetMinRetryDelay <Job> <RetryDelay>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|RetryDelay|Un numero rappresentato in secondi.|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente imposta il ritardo minimo per il processo denominato *myDownloadJob* a 35 secondi.
```
C:\>bitsadmin /SetMinRetryDelay myDownloadJob 35
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)