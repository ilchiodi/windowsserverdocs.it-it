---
title: exec
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f10e28a8da96bc7228af4561fb36824899f2d7a4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725732"
---
# <a name="exec"></a>exec



Esegue un file nel computer locale. Il file può essere un **cmd** script.

## <a name="syntax"></a>Sintassi

```
exec <ScriptFile.cmd>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> ScriptFile. cmd|Specifica il file di script da eseguire.|

## <a name="remarks"></a>Osservazioni

-   Questo comando viene utilizzato per duplicare o ripristinare i dati come parte di un backup o sequenza di ripristino.
-   Se lo script non riesce, viene restituito un errore e viene chiuso DiskShadow.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)