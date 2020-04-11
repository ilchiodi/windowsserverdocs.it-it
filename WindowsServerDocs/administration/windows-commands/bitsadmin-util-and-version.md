---
title: util e versione di Bitsadmin
description: Windows Commands Topic for **Bitsadmin util and Version**, che visualizza la versione del servizio BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c2518eb7a8f15d9a592ed9a77dd67a6f8d8afac
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122462"
---
# <a name="bitsadmin-util-and-version"></a>util e versione di Bitsadmin

Visualizza la versione del servizio BITS (ad esempio, 2,0).

> [!NOTE]
> Questo comando non è supportato da BITS 1,5 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /util /version [/verbose]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /verbose | Usare questa opzione per visualizzare la versione del file per ogni DLL correlata a BITS e per verificare se il servizio BITS è in grado di avviarsi.|

## <a name="examples"></a>Esempi

Nell'esempio seguente la versione del servizio BITS.

```
C:\>bitsadmin /util /version
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)