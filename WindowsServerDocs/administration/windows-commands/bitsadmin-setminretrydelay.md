---
title: bitsadmin setminretrydelay
description: Argomento dei comandi di Windows per Bitsadmin setminretrydelay, che consente di impostare il periodo di tempo minimo, in secondi, che BITS attende dopo aver rilevato un errore temporaneo prima di provare a trasferire il file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb2fe4c6d0e4f90c6ec49fa1da63404393d4f634
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849364"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

Imposta il periodo di tempo minimo, espresso in secondi, di attesa dei bit dopo che si è verificato un errore temporaneo prima di provare a trasferire il file.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetMinRetryDelay <Job> <RetryDelay>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|RetryDelay|Un numero rappresentato in secondi.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente imposta il ritardo minimo per il processo denominato *myDownloadJob* a 35 secondi.
```
C:\>bitsadmin /SetMinRetryDelay myDownloadJob 35
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)