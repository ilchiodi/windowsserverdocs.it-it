---
title: informazioni e cache bitsadmin
description: Argomento i comandi di Windows per **bitsadmin cache e info** -esegue il dump di una voce della cache specifica.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 61ff57b33e575921f2032d4e13a2d9b74accae60
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813322"
---
# <a name="bitsadmin-cache-and-info"></a>informazioni e cache bitsadmin



I dump di una voce della cache specifica.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Cache /Info RecordID [/Verbose] 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Elemento RecordID|Il GUID associato alla voce della cache.|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente esegue il dump la voce della cache con l'elemento RecordID del {6511FB02-E195-40A2-B595-E8E2F8F47702}.
```
C:\>bitsadmin /Cache /Info {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)