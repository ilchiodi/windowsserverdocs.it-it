---
title: dispdiag
description: Windows Commands argomento per dispdiag, che registra le informazioni di visualizzazione in un file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 498294aa6678cc4904880128bca55d7900c91db5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845384"
---
# <a name="dispdiag"></a>dispdiag

Registra le informazioni visualizzate in un file.

## <a name="syntax"></a>Sintassi

```
dispdiag [-testacpi] [-d] [-delay <Seconds>] [-out <FilePath>]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|- testacpi|Esegue il test di diagnostica hotkey. Visualizza il nome della chiave, il codice e il codice di analisi per qualsiasi tasto premuto durante il test.|
|-d|Genera un file di dump con i risultati del test.|
|-ritardo \<secondi >|Ritarda la raccolta di dati in base al tempo specificato in *secondi*.|
|-out \<FilePath >|Specifica il percorso e il nome file per salvare i dati raccolti. Deve essere l'ultimo parametro.|
|-?|Visualizza i parametri di comando disponibili e fornisce supporto per il loro utilizzo.|