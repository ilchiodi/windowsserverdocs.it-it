---
title: bitsadmin gettemporaryname
description: Argomento i comandi di Windows per **bitsadmin gettemporaryname** -restituisce il nome temporaneo del file specificato all'interno del processo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 762a2a5943202b38e94a245b74745e6631e0792d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876712"
---
# <a name="bitsadmin-gettemporaryname"></a>bitsadmin gettemporaryname



Restituisce il nome del file temporaneo del file specificato all'interno del processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetTemporaryName <Job> <file index> 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Indice di file|Inizia da 0|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente restituisce il nome del file temporaneo del file 2 per il processo denominato *myJob*.
```
C:\>bitsadmin /GetTemporaryName myJob 1 
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)