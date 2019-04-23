---
title: bdehdcfg quiet
description: Argomento i comandi di Windows per quiet bdehdcfg - indica bdehdcfg per non visualizzare tutti gli errori e azioni.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0f0d98f6ae76e9bf6357689c97e091766b9645c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865752"
---
# <a name="bdehdcfg-quiet"></a>bdehdcfg: quiet



Informa lo strumento da riga di comando Bdehdcfg che tutte le azioni e gli errori non devono essere visualizzati nell'interfaccia della riga di comando. Per un esempio di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -quiet
```

### <a name="parameters"></a>Parametri

Questo comando non accetta parametri aggiuntivi.

## <a name="remarks"></a>Note

Se durante la preparazione dell'unità avrebbero dovuto essere visualizzate richieste Sì/No (S/N), la risposta presupposta sarà "Sì". Per visualizzare eventuali errori che si sono verificati durante la preparazione dell'unità, esaminare il registro eventi di sistema nel provider di eventi **Microsoft-Windows-BitLocker-DrivePreparationTool**.

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo di **quiet** comando.
```
bdehdcfg -target default -quiet
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)