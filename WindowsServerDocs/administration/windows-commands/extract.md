---
title: extract
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d66682126f1cc3c924c42b4605a537a997e8ac52
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844774"
---
# <a name="extract"></a>extract



## <a name="syntax"></a>Sintassi

```
EXTRACT [/Y] [/A] [/D | /E] [/L dir] cabinet [filename ...]
EXTRACT [/Y] source [newname]
EXTRACT [/Y] /C source destination
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|archivio|File contiene due o più file.|
|filename|Nome del file da estrarre dal file CAB. I caratteri jolly e più nomi di file, separati da spazi vuoti, possono essere utilizzati.|
|origine|File compresso (un file CAB con un solo file).|
|newname|Nuovo nome di file per fornire il file estratto. Se omesso, viene utilizzato il nome originale.|
|/A|Elaborare TUTTI gli archivi. Segue catena partire nel primo file CAB indicato.|
|/C|Copiare i file di origine alla destinazione (per copiare dai dischi DMF).|
|/D|Visualizzare directory CAB (da utilizzare con il nome file per evitare di estrazione).|
|/E|Extract (usare anziché *.* Per estrarre tutti i file).|
|/L dir|Posizione in cui i file estratti (valore predefinito è la directory corrente).|
|/Y|Non chiedere conferma prima di sovrascrivere un file esistente.|

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)