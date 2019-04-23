---
title: bitsadmin getreplydata
description: Argomento i comandi di Windows per **bitsadmin getreplydata** -recupera i dati di risposta del server in formato esadecimale.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 819f97d5-b255-4b2d-9f63-0daa73915434
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78d70a44d6881568c8d92db145fdf22a260ee8af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883822"
---
# <a name="bitsadmin-getreplydata"></a>bitsadmin getreplydata

Recupera i dati di risposta del server in formato esadecimale.

**BITS 1.2 e versioni precedenti**: Non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetReplyData <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Valido solo per i processi di caricamento-risposta.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera i dati di risposta per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetReplyData myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)