---
title: bitsadmin setreplyfilename
description: Argomento dei comandi di Windows per **BITSAdmin setreplyfilename** -specificare il percorso del file che contiene la risposta del server.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a490b5bc565549d096b6f43f42758f77570fcb26
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380420"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

Specificare il percorso del file che contiene la risposta del server.

**BITS 1,2 e versioni precedenti**: Non supportati.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetReplyFileName <Job> <Path>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|`Path`|Località in cui inserire la risposta del server|

## <a name="remarks"></a>Note

Valido solo per i processi di caricamento-risposta.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene impostato il nome del file di risposta pathfor il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /SetReplyFileName myDownloadJob c:\reply
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)