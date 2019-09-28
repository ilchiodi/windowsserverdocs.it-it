---
title: dispdiag
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9b640883a207648d2ef6c9a7d6e5366cd0bb384c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377761"
---
# <a name="dispdiag"></a>dispdiag



Registra le informazioni visualizzate in un file.

## <a name="syntax"></a>Sintassi

```
dispdiag [-testacpi] [-d] [-delay <Seconds>] [-out <FilePath>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|- testacpi|Esegue il test di diagnostica hotkey. Visualizza il nome della chiave, il codice e il codice di analisi per qualsiasi tasto premuto durante il test.|
|-d|Genera un file di dump con i risultati del test.|
|-Delay \<Seconds >|Ritarda la raccolta di dati in base al tempo specificato in *secondi*.|
|-out \<FilePath >|Specifica il percorso e il nome file per salvare i dati raccolti. Deve essere l'ultimo parametro.|
|-?|Visualizza i parametri di comando disponibili e fornisce supporto per il loro utilizzo.|