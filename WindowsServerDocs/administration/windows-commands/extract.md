---
title: extract
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 967f08e271019cc33970419179c9ddbf902b1882
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377257"
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
|source|File compresso (un file CAB con un solo file).|
|newname|Nuovo nome di file per fornire il file estratto. Se omesso, viene utilizzato il nome originale.|
|/A|Elaborare TUTTI gli archivi. Segue catena partire nel primo file CAB indicato.|
|/C|Copiare i file di origine alla destinazione (per copiare dai dischi DMF).|
|/D|Visualizzare directory CAB (da utilizzare con il nome file per evitare di estrazione).|
|/E|Extract (usare anziché *.* Per estrarre tutti i file).|
|/L dir|Posizione in cui i file estratti (valore predefinito è la directory corrente).|
|/Y|Non chiedere conferma prima di sovrascrivere un file esistente.|

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)