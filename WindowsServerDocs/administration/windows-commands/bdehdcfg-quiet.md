---
title: BdeHdCfg silenzioso
description: Windows Commands Topic for **BdeHdCfg quiet**, che indica a BdeHdCfg di non visualizzare tutte le azioni e gli errori.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e9c24d8861476e6c1578af8245236d699b6ef6db
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851044"
---
# <a name="bdehdcfg-quiet"></a>BdeHdCfg: non interattiva

Informa lo strumento da riga di comando BdeHdCfg che tutte le azioni e gli errori non devono essere visualizzati nell'interfaccia della riga di comando. Per un esempio di come è possibile usare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -quiet
```

#### <a name="parameters"></a>Parametri

Questo comando non accetta parametri aggiuntivi.

## <a name="remarks"></a>Note

Se durante la preparazione dell'unità avrebbero dovuto essere visualizzate richieste Sì/No (S/N), la risposta presupposta sarà "Sì". Per visualizzare eventuali errori che si sono verificati durante la preparazione dell'unità, esaminare il registro eventi di sistema nel provider di eventi **Microsoft-Windows-BitLocker-DrivePreparationTool**.

## <a name="examples"></a><a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo del comando **quiet** .

```
bdehdcfg -target default -quiet
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [BdeHdCfg](bdehdcfg.md)