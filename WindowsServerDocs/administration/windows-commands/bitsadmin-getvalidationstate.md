---
title: getvalidationstate Bitsadmin
description: "Argomento i comandi di Windows per **bitsadmin getvalidationstate** -segnala lo stato di convalida del contenuto del file specificato all'interno del processo. "
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8abff3fc9fddb9cff1758739fdc540a9c945efe2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879162"
---
# <a name="bitsadmin-getvalidationstate"></a>getvalidationstate Bitsadmin



Segnala lo stato di convalida del contenuto del file specificato all'interno del processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetValidationState <Job> <file index> 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Indice di file|Inizia da 0|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente ottiene lo stato di convalida del contenuto del file 2 all'interno del processo denominato *myJob*.
```
C:\>bitsadmin /GetValidationState myJob 1
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)