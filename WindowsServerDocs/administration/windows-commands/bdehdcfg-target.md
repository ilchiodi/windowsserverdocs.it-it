---
title: bdehdcfg destinazione
description: Argomento i comandi di Windows per la destinazione bdehdcfg - prepara una partizione per l'uso come un'unità di sistema da un ripristino di BitLocker e Windows.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f8d180974f480b4c40532dab529ad49dcc33540d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881532"
---
# <a name="bdehdcfg-target"></a>bdehdcfg: target



Prepara una partizione per l'uso come un'unità di sistema da BitLocker e il ripristino di Windows. Per impostazione predefinita, questa partizione viene creata senza una lettera di unità. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|default|Indica che lo strumento da riga di comando seguirà lo stesso processo della configurazione guidata BitLocker.|
|unallocated|Crea la partizione di sistema dallo spazio non allocato disponibile sul disco.|
|\<LetteraUnità > riduzione|Riduce l'unità specificata della quantità necessaria per creare una partizione di sistema attiva. Per utilizzare questo comando, sull'unità specificata deve essere presente almeno il 5% di spazio disponibile.|
|\<LetteraUnità > merge|Utilizza l'unità specificata come partizione di sistema attiva. L'unità del sistema operativo non può essere utilizzata come destinazione per l'unione.|

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrata con la **destinazione** comando per specificare un'unità esistente (P) che l'unità del sistema.
```
bdehdcfg -target P: merge
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)