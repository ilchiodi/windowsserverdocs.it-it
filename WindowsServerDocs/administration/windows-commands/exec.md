---
title: Exec
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d39fbf948050dd00f329e461c34c2365030cb05d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844994"
---
# <a name="exec"></a>Exec



Esegue un file nel computer locale. Il file può essere un **cmd** script.

## <a name="syntax"></a>Sintassi

```
exec <ScriptFile.cmd>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<ScriptFile. cmd >|Specifica il file di script da eseguire.|

## <a name="remarks"></a>Note

-   Questo comando viene utilizzato per duplicare o ripristinare i dati come parte di un backup o sequenza di ripristino.
-   Se lo script non riesce, viene restituito un errore e viene chiuso DiskShadow.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)