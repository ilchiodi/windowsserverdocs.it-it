---
title: Exec
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ecdfd05b8abefb35946b783daaa3220a6713a38d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882922"
---
# <a name="exec"></a>Exec



Esegue un file nel computer locale. Il file può essere un **cmd** script.

## <a name="syntax"></a>Sintassi

```
exec <ScriptFile.cmd>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<ScriptFile.cmd>|Specifica il file di script da eseguire.|

## <a name="remarks"></a>Note

-   Questo comando viene utilizzato per duplicare o ripristinare i dati come parte di un backup o sequenza di ripristino.
-   Se lo script non riesce, viene restituito un errore e viene chiuso DiskShadow.

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)