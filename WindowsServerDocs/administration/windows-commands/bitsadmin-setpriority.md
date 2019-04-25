---
title: bitsadmin setpriority
description: Argomento i comandi di Windows per **bitsadmin setpriority** -imposta la priorità del processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 072f22ae8c928d427104062b8cbf0f8f42ac4416
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882212"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority



Imposta la priorità del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetPriority <Job> <Priority>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Priority|Uno dei valori seguenti:</br>-IN PRIMO PIANO</br>-ALTO</br>-NORMALE</br>-BASSO|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente imposta la priorità per il processo denominato *myDownloadJob* Normal.
```
C:\>bitsadmin /SetPriority myDownloadJob NORMAL
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)