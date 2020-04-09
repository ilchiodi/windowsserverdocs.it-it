---
title: bitsadmin setreplyfilename
description: Windows Commands Topic for Bitsadmin setreplyfilename, che specifica il percorso del file che contiene la risposta del server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd45174a7deac89cc943fb19d544e372c0198139
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849184"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

Specifica il percorso del file che contiene la risposta del server.

**BITS 1,2 e versioni precedenti**: non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetReplyFileName <Job> <Path>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Percorso|Località in cui inserire la risposta del server|

## <a name="remarks"></a>Note

Valido solo per i processi di caricamento-risposta.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene impostato il nome del file di risposta pathfor il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /SetReplyFileName myDownloadJob c:\reply
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)