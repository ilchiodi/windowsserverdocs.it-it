---
title: dispdiag
description: Argomento di riferimento per dispdiag, che registra le informazioni visualizzate in un file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9f44e261b9c46157fb3e6bb7f9105af2480a60b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719403"
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
|-secondi \<di ritardo>|Ritarda la raccolta di dati in base al tempo specificato in *secondi*.|
|-out \<filepath>|Specifica il percorso e il nome file per salvare i dati raccolti. Deve essere l'ultimo parametro.|
|-?|Visualizza i parametri di comando disponibili e fornisce supporto per il loro utilizzo.|