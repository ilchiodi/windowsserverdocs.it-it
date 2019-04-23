---
title: bitsadmin getbytestotal
description: Argomento i comandi di Windows per **bitsadmin getbytestotal** -recupera le dimensioni del processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d7a33b02dbdea63e87bd8ce56a2d50e15ca19d05
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882862"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal



Recupera la dimensione del processo specificato

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetBytesTotal <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera la dimensione del processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetBytesTotal myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)