---
title: BdeHdCfg silenzioso
description: 'Argomento dei comandi di Windows per BdeHdCfg quiet: indica a BdeHdCfg di non visualizzare tutte le azioni e gli errori.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d59a14e34200e3fa8e18e36e166ef62ceca1afe7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382222"
---
# <a name="bdehdcfg-quiet"></a>BdeHdCfg: non interattiva



Informa lo strumento da riga di comando BdeHdCfg che tutte le azioni e gli errori non devono essere visualizzati nell'interfaccia della riga di comando. Per un esempio di come è possibile usare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -quiet
```

### <a name="parameters"></a>Parametri

Questo comando non accetta parametri aggiuntivi.

## <a name="remarks"></a>Note

Se durante la preparazione dell'unità avrebbero dovuto essere visualizzate richieste Sì/No (S/N), la risposta presupposta sarà "Sì". Per visualizzare eventuali errori che si sono verificati durante la preparazione dell'unità, esaminare il registro eventi di sistema nel provider di eventi **Microsoft-Windows-BitLocker-DrivePreparationTool**.

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo del comando **quiet** .
```
bdehdcfg -target default -quiet
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [BdeHdCfg](bdehdcfg.md)