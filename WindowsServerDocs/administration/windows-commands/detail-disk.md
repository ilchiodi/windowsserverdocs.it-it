---
title: disco dettagli
description: Argomento di riferimento per il comando detail disk, che consente di visualizzare le proprietà del disco selezionato e i volumi su tale disco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 358d6762f382dc8461c73cbd557a906eb5189c6f
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993072"
---
# <a name="detail-disk"></a>disco dettagli

Visualizza le proprietà del disco selezionato e i volumi presenti sul disco. Prima di iniziare, è necessario selezionare un disco affinché l'operazione abbia esito positivo. Utilizzare il [disco selezionare](select-disk.md) comando per selezionare un disco e spostare lo stato attivo a esso. Se si seleziona un disco rigido virtuale (VHD), questo comando visualizzerà il tipo di bus del disco come *virtuale*.

## <a name="syntax"></a>Sintassi

```
detail disk
```

## <a name="examples"></a>Esempi

Per visualizzare le proprietà del disco selezionato e informazioni sui volumi del disco, digitare:

```
detail disk
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando detail](detail.md)
