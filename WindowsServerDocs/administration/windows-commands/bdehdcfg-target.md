---
title: destinazione BdeHdCfg
description: "Argomento dei comandi di Windows per la destinazione BdeHdCfg: prepara una partizione per l'uso come unità di sistema da BitLocker e ripristino di Windows."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f761d25d-8349-4ac7-ac46-6bb340a4348f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2fb0a1daa257ef2c9f1cd77b88e5ef14f84a0dfa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382185"
---
# <a name="bdehdcfg-target"></a>BdeHdCfg: destinazione



Prepara una partizione per l'utilizzo come unità di sistema da BitLocker e ripristino di Windows. Per impostazione predefinita, questa partizione viene creata senza una lettera di unità. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|default|Indica che lo strumento da riga di comando seguirà lo stesso processo della configurazione guidata BitLocker.|
|unallocated|Crea la partizione di sistema dallo spazio non allocato disponibile sul disco.|
|\<DriveLetter > compattazione|Riduce l'unità specificata della quantità necessaria per creare una partizione di sistema attiva. Per utilizzare questo comando, sull'unità specificata deve essere presente almeno il 5% di spazio disponibile.|
|\<DriveLetter > merge|Utilizza l'unità specificata come partizione di sistema attiva. L'unità del sistema operativo non può essere utilizzata come destinazione per l'unione.|

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo del comando **target** per designare un'unità esistente (P) come unità di sistema.
```
bdehdcfg -target P: merge
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [BdeHdCfg](bdehdcfg.md)