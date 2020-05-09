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
ms.openlocfilehash: eac3749304a06ea4cc11bf90a3220f5e24f9b5ae
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993007"
---
# <a name="detail-volume"></a>volume dettaglio

Visualizza i dischi in cui risiede il volume corrente. Prima di iniziare, è necessario selezionare un volume per la riuscita dell'operazione. Utilizzare il [Selezionare volume](select-volume.md) comando per selezionare un volume e spostare lo stato attivo a esso. I dettagli del volume non sono applicabili ai volumi di sola lettura, ad esempio un'unità DVD-ROM o CD-ROM.

## <a name="syntax"></a>Sintassi

```
detail volume
```

## <a name="examples"></a>Esempi

Per visualizzare tutti i dischi in cui risiede il volume corrente, digitare:

```
detail volume
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [select volume](select-volume.md)

- [comando detail](detail.md)