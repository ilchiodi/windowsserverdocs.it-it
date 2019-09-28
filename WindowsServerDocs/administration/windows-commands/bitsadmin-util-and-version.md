---
title: util e versione di Bitsadmin
description: Windows Commands Topic for **Bitsadmin util and Version** -Visualizza la versione del servizio BITS.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 495ef17bbf6f39f20f6729b64de4b4bec0f9a3c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380204"
---
# <a name="bitsadmin-util-and-version"></a>util e versione di Bitsadmin

Visualizza la versione del servizio BITS (ad esempio, 2,0).

**BITSAdmin 1,5 e versioni precedenti**: Non supportati.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>Note

L'opzione **verbose** esegue le operazioni seguenti:
-   Visualizza la versione del file per ogni DLL correlate a BITS
-   Verifica che può essere avviato il servizio BITS
-   Visualizza i valori dei criteri di gruppo BITS (solo Windows Vista)

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente la versione del servizio BITS.
```
C:\>bitsadmin /Util /Version
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)