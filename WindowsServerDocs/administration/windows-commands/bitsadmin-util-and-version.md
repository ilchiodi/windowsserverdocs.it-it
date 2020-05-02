---
title: bitsadmin util e version
description: Argomento di riferimento per il comando util e Version di Bitsadmin, che visualizza la versione del servizio BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 20c3db6e6fcd5ef3d00287f36c9f9624ab5224dd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707596"
---
# <a name="bitsadmin-util-and-version"></a>bitsadmin util e version

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

Per visualizzare la versione del servizio BITS.

```
bitsadmin /util /version
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando util Bitsadmin](bitsadmin-util.md)

- [comando Bitsadmin](bitsadmin.md)
