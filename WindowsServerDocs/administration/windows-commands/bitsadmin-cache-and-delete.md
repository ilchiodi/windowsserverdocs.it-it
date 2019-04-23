---
title: eliminazione e cache bitsadmin
description: Argomento i comandi di Windows per **bitsadmin memorizzare nella cache ed eliminare** -Elimina una voce della cache specifica.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63b82cbbadebf2c4e36f2c76076b329787d7b1b5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852512"
---
# <a name="bitsadmin-cache-and-delete"></a>eliminazione e cache bitsadmin



Elimina una voce della cache specifica.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Cache /Delete RecordID 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Elemento RecordID|Il GUID associato alla voce della cache.|

## <a name="BKMK_examples"></a>Esempi

L'esempio seguente elimina la voce della cache con l'elemento RecordID del {6511FB02-E195-40A2-B595-E8E2F8F47702}.
```
C:\>bitsadmin /Cache /Delete {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)