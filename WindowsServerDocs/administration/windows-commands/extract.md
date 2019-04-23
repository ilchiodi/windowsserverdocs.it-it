---
title: extract
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9113c34b61b98fb738bc0aff03193ab73b1abbd7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882312"
---
# <a name="extract"></a>extract



## <a name="syntax"></a>Sintassi

```
EXTRACT [/Y] [/A] [/D | /E] [/L dir] cabinet [filename ...]
EXTRACT [/Y] source [newname]
EXTRACT [/Y] /C source destination
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|archivio|File contiene due o più file.|
|nomefile|Nome del file da estrarre dal file CAB. I caratteri jolly e più nomi di file, separati da spazi vuoti, possono essere utilizzati.|
|origine|File compresso (un file CAB con un solo file).|
|newname|Nuovo nome di file per fornire il file estratto. Se omesso, viene utilizzato il nome originale.|
|/A|Elaborare TUTTI gli archivi. Segue catena partire nel primo file CAB indicato.|
|/C|Copiare i file di origine alla destinazione (per copiare dai dischi DMF).|
|/D|Visualizzare directory CAB (da utilizzare con il nome file per evitare di estrazione).|
|/E|Estrarre (utilizzare invece di *.* Per estrarre tutti i file).|
|/L dir|Posizione in cui i file estratti (valore predefinito è la directory corrente).|
|/Y|Non chiedere conferma prima di sovrascrivere un file esistente.|

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)