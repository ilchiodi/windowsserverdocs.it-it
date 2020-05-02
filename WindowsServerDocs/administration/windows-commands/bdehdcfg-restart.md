---
title: riavvio BdeHdCfg
description: Argomento di riferimento per il comando BdeHdCfg restart, che indica a BdeHdCfg che il computer deve essere riavviato dopo la conclusione della preparazione dell'unità.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 684a6a24fe78c0a23ba954981121c7bd99ac56fb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718628"
---
# <a name="bdehdcfg-restart"></a>BdeHdCfg: riavvio

Informa lo strumento da riga di comando BdeHdCfg che il computer deve essere riavviato dopo la conclusione della preparazione dell'unità. Se altri utenti sono connessi al computer e non si specifica il comando **quiet** , viene visualizzato un messaggio di richiesta per confermare che il computer deve essere riavviato.

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -restart
```

#### <a name="parameters"></a>Parametri

Questo comando non ha parametri aggiuntivi.

## <a name="examples"></a>Esempi

Per utilizzare il comando **Restart** :

```
bdehdcfg -target default -restart
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
