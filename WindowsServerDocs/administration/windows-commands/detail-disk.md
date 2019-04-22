---
title: disco dettaglio
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c7a5063edf3cb2e190e8aec957e1b571c1f15bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819102"
---
# <a name="detail-disk"></a>disco dettaglio



Visualizza le proprietà del disco selezionato e i volumi presenti sul disco.

## <a name="syntax"></a>Sintassi

```
detail disk
```

## <a name="remarks"></a>Note

-   Per eseguire questa operazione, è necessario selezionare un disco. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.
-   Se il disco selezionato è un disco rigido virtuale (VHD), **disco dettaglio** segnala il tipo di bus del disco come virtuale.

## <a name="BKMK_examples"></a>Esempi

Per visualizzare le proprietà del disco selezionato e informazioni sui volumi del disco, digitare:
```
detail disk
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

