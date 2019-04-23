---
title: bitsadmin gettype
description: Argomento i comandi di Windows per **gettype bitsadmin** -recupera il tipo di processo del processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff0118f14acbf4e9f37c02e660bd9c7f6e8d0f70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879432"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype



Recupera il tipo di processo del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetType <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Il tipo può essere SCARICATI, caricare, caricamento-risposta o UNKNOWN.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera il tipo di processo per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetType myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)