---
title: newdriveletter BdeHdCfg
description: Windows Commands argomento for **BdeHdCfg newdriveletter**, che assegna una nuova lettera di unità alla parte di un'unità utilizzata come unità di sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a8757e7d0684912525817708fbe34953b049582
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851054"
---
# <a name="bdehdcfg-newdriveletter"></a>BdeHdCfg: newdriveletter

Assegna una nuova lettera di unità alla parte di un'unità utilizzata come unità di sistema. Per un esempio di come è possibile usare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ---------| ----------- |
|`<DriveLetter>`|Definisce la lettera di unità che verrà assegnata all'unità di destinazione specificata.|

## <a name="remarks"></a>Note

Come procedura consigliata, è consigliabile non assegnare una lettera di unità all'unità di sistema.

## <a name="examples"></a><a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene mostrata l'unità predefinita a cui viene assegnata la lettera di unità P.

```
bdehdcfg -target default -newdriveletter P:
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [BdeHdCfg](bdehdcfg.md)