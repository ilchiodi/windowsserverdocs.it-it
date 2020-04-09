---
title: util e versione di Bitsadmin
description: Windows Commands Topic for Bitsadmin util and Version, che visualizza la versione del servizio BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 087cc1033166ab93e7496caaa7335433cafd6249
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848834"
---
# <a name="bitsadmin-util-and-version"></a>util e versione di Bitsadmin

Visualizza la versione del servizio BITS (ad esempio, 2,0).

**BITSAdmin 1,5 e versioni precedenti**: non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>Note

L'opzione **verbose** esegue le operazioni seguenti:
-   Visualizza la versione del file per ogni DLL correlate a BITS
-   Verifica che può essere avviato il servizio BITS
-   Visualizza i valori dei criteri di gruppo BITS (solo Windows Vista)

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente la versione del servizio BITS.
```
C:\>bitsadmin /Util /Version
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)