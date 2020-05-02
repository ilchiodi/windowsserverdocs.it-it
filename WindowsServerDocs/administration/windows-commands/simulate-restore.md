---
title: simula ripristino
description: Argomento di riferimento per simulate Restore, che consente di testare il coinvolgimento del writer nelle sessioni di ripristino nel computer senza inviare eventi di preripristino o di ripristino postripristino ai writer.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bab6c56cddc1d2ac95dc70205b0990b82fbfd12
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721783"
---
# <a name="simulate-restore"></a>Simula ripristino

Verifica il coinvolgimento del writer nelle sessioni di ripristino nel computer senza inviare eventi di **preripristino** o di **ripristino** a writer.

## <a name="syntax"></a>Sintassi

```
simulate restore
```

## <a name="remarks"></a>Osservazioni

-   **Simulate Restore** viene usato per verificare se il ripristino con Writer può essere eseguito correttamente.
-   Prima di poter usare **simulate Restore**, è necessario caricare un file di metadati DiskShadow usando il comando **Load Metadata** . Verranno caricati i writer e i componenti selezionati per il ripristino.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)