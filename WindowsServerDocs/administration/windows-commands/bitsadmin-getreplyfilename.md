---
title: bitsadmin getreplyfilename
description: Argomento i comandi di Windows per **bitsadmin getreplyfilename** -Ottiene il percorso del file che contiene la risposta del server.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85447184-1732-4816-a365-2e3599551bf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46130700e9ac7e2d0076b368712e5dcb3f02ba2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862152"
---
# <a name="bitsadmin-getreplyfilename"></a>bitsadmin getreplyfilename

Ottiene il percorso del file che contiene la risposta del server.

**BITS 1.2 e versioni precedenti**: Non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetReplyFileName <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Valido solo per i processi di caricamento-risposta.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera il nome del file di risposta per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetReplyFileName myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)