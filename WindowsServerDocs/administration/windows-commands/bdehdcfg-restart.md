---
title: riavvio BdeHdCfg
description: Windows Commands Topic for **BdeHdCfg restart**, che indica a BdeHdCfg che il computer deve essere riavviato al termine della preparazione dell'unità.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ae6f8d31c09feddf8f994c28d34e4e1b08cc322
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851034"
---
# <a name="bdehdcfg-restart"></a>BdeHdCfg: riavvio

Informa lo strumento da riga di comando BdeHdCfg che il computer deve essere riavviato dopo la conclusione della preparazione dell'unità. Per un esempio di come è possibile usare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -restart
```

#### <a name="parameters"></a>Parametri

Questo comando non accetta parametri aggiuntivi.

## <a name="remarks"></a>Note

Se altri utenti sono connessi al computer e non si specifica il comando **quiet** , verrà visualizzato un messaggio di richiesta per confermare che il computer deve essere riavviato.

## <a name="examples"></a><a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo del comando **Restart** .

```
bdehdcfg -target default -restart
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [BdeHdCfg](bdehdcfg.md)