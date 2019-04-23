---
title: setmaxdownloadtime Bitsadmin
description: Argomento i comandi di Windows per **bitsadmin setmaxdownloadtime** -imposta il timeout del download in secondi.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f13b44429bec2718af1a648f273fead18d4e9e08
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830992"
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

[Chiave sintassi della riga di comando](command-line-syntax-key.md)