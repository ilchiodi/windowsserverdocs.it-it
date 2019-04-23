---
title: dispdiag
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c96c70aac1b3329e050fa8b02743e61fed44d15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831462"
---
# <a name="dispdiag"></a>dispdiag



I log di visualizzano le informazioni in un file.

## <a name="syntax"></a>Sintassi

```
dispdiag [-testacpi] [-d] [-delay <Seconds>] [-out <FilePath>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|- testacpi|Viene eseguito il test di diagnostica di tasti di scelta rapida. Visualizza il nome della chiave, codice e l'analisi codice per un tasto premuto durante il test.|
|-d|Genera un file di dump dei risultati dei test.|
|-delay \<secondi >|Ritarda la raccolta dei dati per un periodo di tempo specificato in *secondi*.|
|-out \<FilePath>|Specifica percorso e nome file per salvare i dati raccolti. Deve essere l'ultimo parametro.|
|-?|Visualizza i parametri di comando disponibili e fornisce supporto per il loro utilizzo.|