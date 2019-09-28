---
title: Exec
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 514503e4920e16ba6778185af32f925541805223
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377428"
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
|@no__t -0ScriptFile. cmd >|Specifica il file di script da eseguire.|

## <a name="remarks"></a>Note

-   Questo comando viene utilizzato per duplicare o ripristinare i dati come parte di un backup o sequenza di ripristino.
-   Se lo script non riesce, viene restituito un errore e viene chiuso DiskShadow.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)