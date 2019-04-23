---
title: versione e bitsadmin util
description: Argomento i comandi di Windows per **util bitsadmin e versione** -Visualizza la versione del servizio BITS.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2e768ec5ae43fc17c480b9deede698cca01c6291
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882872"
---
# <a name="bitsadmin-util-and-version"></a>versione e bitsadmin util

Visualizza la versione del servizio BITS (ad esempio, 2.0).

**BITSAdmin 1.5 e versioni precedenti**: Non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>Note

Il **Verbose** commutatore esegue le operazioni seguenti:
-   Visualizza la versione del file per ogni DLL correlate a BITS
-   Verifica che può essere avviato il servizio BITS
-   Visualizza i valori dei criteri di gruppo BITS (solo Windows Vista)

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente la versione del servizio BITS.
```
C:\>bitsadmin /Util /Version
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)