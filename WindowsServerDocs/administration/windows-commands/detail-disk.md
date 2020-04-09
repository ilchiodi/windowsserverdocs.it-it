---
title: disco dettagli
description: Windows Commands Topic for detail disk, che visualizza le proprietà del disco selezionato e i volumi su tale disco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d0768d45c0f56ba549ff54064c4e74ae3048e41
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846454"
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

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per visualizzare le proprietà del disco selezionato e informazioni sui volumi del disco, digitare:
```
detail disk
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

