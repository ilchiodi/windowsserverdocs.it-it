---
title: volume dettaglio
description: Argomento di riferimento per il volume dei dettagli, che consente di visualizzare i dischi in cui risiede il volume corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 38f2bc75-2ed6-4e80-aa74-ab83133db1cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2958c82b1dfc3b99d0e15690ef9857e7d83b244f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719613"
---
# <a name="detail-volume"></a>volume dettaglio

Visualizza i dischi in cui risiede il volume corrente.

## <a name="syntax"></a>Sintassi

```
detail volume
```

## <a name="remarks"></a>Osservazioni

-   Per eseguire questa operazione, è necessario selezionare un volume. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.
-   I dettagli del volume non sono applicabili ai volumi di sola lettura, ad esempio un'unità DVD-ROM o CD-ROM.

## <a name="examples"></a>Esempi

Per visualizzare tutti i dischi in cui risiede il volume corrente, digitare:
```
detail volume
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

