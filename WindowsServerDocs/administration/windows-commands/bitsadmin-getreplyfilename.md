---
title: bitsadmin getreplyfilename
description: 'Windows Commands Topic for **BITSAdmin getreplyfilename** : Ottiene il percorso del file che contiene la risposta del server.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 96b77e9bd19cdc094e6b025e143b05aff7bc60d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381271"
---
# <a name="bitsadmin-getreplyfilename"></a>bitsadmin getreplyfilename

Ottiene il percorso del file che contiene la risposta del server.

**BITS 1,2 e versioni precedenti**: Non supportati.

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

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)