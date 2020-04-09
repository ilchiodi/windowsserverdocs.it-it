---
title: setmaxdownloadtime Bitsadmin
description: Windows Commands Topic for Bitsadmin setmaxdownloadtime, che imposta il timeout di download in secondi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd44011cd14d575a9c3798ede45641fac4c3dc75
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849374"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>setmaxdownloadtime Bitsadmin

Imposta il timeout del download in secondi.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetMaxDownloadTime <Job> <Timeout>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Timeout|Il timeout in secondi|

## <a name="remarks"></a>Note

-   N/D

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente imposta il timeout per il processo denominato *myDownloadJob* su 10 secondi.
```
C:\>bitsadmin /SetMaxDownloadTime myDownloadJob 10
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)