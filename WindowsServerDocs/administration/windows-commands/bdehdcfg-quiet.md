---
title: BdeHdCfg silenzioso
description: Argomento di riferimento per il comando BdeHdCfg quiet, che indica a BdeHdCfg di non visualizzare tutte le azioni e gli errori.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afb7a73899259b0f3823941ece014ea85568a4ce
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718633"
---
# <a name="bdehdcfg-quiet"></a>BdeHdCfg: non interattiva

Informa lo strumento da riga di comando BdeHdCfg che tutte le azioni e gli errori non devono essere visualizzati nell'interfaccia della riga di comando. Qualsiasi richiesta Sì/No (Y/N) visualizzata durante la preparazione dell'unità presuppone una risposta "Sì". Per visualizzare eventuali errori che si sono verificati durante la preparazione dell'unità, esaminare il registro eventi di sistema nel provider di eventi **Microsoft-Windows-BitLocker-DrivePreparationTool**.

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -quiet
```

#### <a name="parameters"></a>Parametri

Questo comando non ha parametri aggiuntivi.

## <a name="examples"></a>Esempi

Per utilizzare il comando **quiet** :

```
bdehdcfg -target default -quiet
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
