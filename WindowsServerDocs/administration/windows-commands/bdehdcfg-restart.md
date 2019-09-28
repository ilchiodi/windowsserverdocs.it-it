---
title: riavvio BdeHdCfg
description: "Argomento dei comandi di Windows per il riavvio di BdeHdCfg: indica a BdeHdCfg che il computer deve essere riavviato dopo la conclusione della preparazione dell'unità."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6c4e48b051f567c98ea679feaa22f995982a899
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382206"
---
# <a name="bdehdcfg-restart"></a>BdeHdCfg: riavvio



Informa lo strumento da riga di comando BdeHdCfg che il computer deve essere riavviato dopo la conclusione della preparazione dell'unità. Per un esempio di come è possibile usare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -restart
```

### <a name="parameters"></a>Parametri

Questo comando non accetta parametri aggiuntivi.

## <a name="remarks"></a>Note

Se altri utenti sono connessi al computer e non si specifica il comando **quiet** , verrà visualizzato un messaggio di richiesta per confermare che il computer deve essere riavviato.

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo del comando **Restart** .
```
bdehdcfg -target default -restart
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [BdeHdCfg](bdehdcfg.md)