---
title: bitsadmin setminretrydelay
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 640492cf690a934e3e3b8d0ecf8ca7a0d6a7dc2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813082"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

Imposta la lunghezza minima di tempo, espresso in secondi, che BITS è in attesa dopo che si verifichi un errore temporaneo prima di tentare il trasferimento del file.

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

[Chiave sintassi della riga di comando](command-line-syntax-key.md)