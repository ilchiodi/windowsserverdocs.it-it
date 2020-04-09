---
title: bitsadmin setpriority
description: Windows Commands argomento per Bitsadmin sepriority, che imposta la priorità del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d007c62402a3d70910e1c79fab5c406295a63a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849214"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority

Imposta la priorità del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetPriority <Job> <Priority>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Priorità|Uno dei valori seguenti:</br>-IN PRIMO PIANO</br>-ALTO</br>-NORMALE</br>-BASSO|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente imposta la priorità per il processo denominato *myDownloadJob* Normal.
```
C:\>bitsadmin /SetPriority myDownloadJob NORMAL
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)