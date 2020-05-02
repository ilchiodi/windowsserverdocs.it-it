---
title: disco dettagli
description: Argomento di riferimento per il disco dei dettagli, che consente di visualizzare le proprietà del disco selezionato e i volumi su tale disco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a746506d6c9609e3214dbd48e5fa91f52d16ab4d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82710512"
---
# <a name="detail-disk"></a>disco dettagli

Visualizza le proprietà del disco selezionato e i volumi presenti sul disco.

## <a name="syntax"></a>Sintassi

```
detail disk
```

## <a name="remarks"></a>Osservazioni

-   Per eseguire questa operazione, è necessario selezionare un disco. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.
-   Se il disco selezionato è un disco rigido virtuale (VHD), il **disco dei dettagli** segnala il tipo di bus del disco come virtuale.

## <a name="examples"></a>Esempi

Per visualizzare le proprietà del disco selezionato e informazioni sui volumi del disco, digitare:
```
detail disk
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

