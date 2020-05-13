---
title: dispdiag
description: Argomento di riferimento per il comando dispdiag, che registra le informazioni visualizzate in un file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff4e3690ec3b2c9d473f05027d5637eda124d0ba
ms.sourcegitcommit: aed942d11f1a361fc1d17553a4cf190a864d1268
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/12/2020
ms.locfileid: "83235223"
---
# <a name="dispdiag"></a>dispdiag

Registra le informazioni visualizzate in un file.

## <a name="syntax"></a>Sintassi

```
dispdiag [-testacpi] [-d] [-delay <seconds>] [-out <filepath>]
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| - testacpi | Esegue il test di diagnostica hotkey. Visualizza il nome della chiave, il codice e il codice di analisi per qualsiasi tasto premuto durante il test. |
| -d | Genera un file di dump con i risultati del test. |
| -ritardo`<seconds>` | Ritarda la raccolta di dati in base al tempo specificato in *secondi*. |
| -out`<filepath>`  | Specifica il percorso e il nome file per salvare i dati raccolti. Deve essere l'ultimo parametro. |
| -? | Visualizza i parametri di comando disponibili e fornisce supporto per il loro utilizzo. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
