---
title: volume dettaglio
description: Windows Commands argomento for detail volume, che consente di visualizzare i dischi in cui risiede il volume corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 38f2bc75-2ed6-4e80-aa74-ab83133db1cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f0441beba769066c593e77b55b9266918e5f778
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846322"
---
# <a name="detail-volume"></a>volume dettaglio

Visualizza i dischi in cui risiede il volume corrente.

## <a name="syntax"></a>Sintassi

```
detail volume
```

## <a name="remarks"></a>Note

-   Per eseguire questa operazione, è necessario selezionare un volume. Utilizzare il **Selezionare volume** comando per selezionare un volume e spostare lo stato attivo a esso.
-   I dettagli del volume non sono applicabili ai volumi di sola lettura, ad esempio un'unità DVD-ROM o CD-ROM.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per visualizzare tutti i dischi in cui risiede il volume corrente, digitare:
```
detail volume
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

