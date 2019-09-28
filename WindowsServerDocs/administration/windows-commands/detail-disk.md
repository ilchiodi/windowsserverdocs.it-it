---
title: disco dettagli
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ff78a3f9e27cde35a7e19bdf1565c515a127261b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378580"
---
# <a name="detail-disk"></a>disco dettagli



Visualizza le proprietà del disco selezionato e i volumi presenti sul disco.

## <a name="syntax"></a>Sintassi

```
detail disk
```

## <a name="remarks"></a>Note

-   Per eseguire questa operazione, è necessario selezionare un disco. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.
-   Se il disco selezionato è un disco rigido virtuale (VHD), il **disco dei dettagli** segnala il tipo di bus del disco come virtuale.

## <a name="BKMK_examples"></a>Esempi

Per visualizzare le proprietà del disco selezionato e informazioni sui volumi del disco, digitare:
```
detail disk
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

