---
title: Simula ripristino
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6652fea4e74c706fcc03b8a547fab771a7c0191
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370876"
---
# <a name="simulate-restore"></a>Simula ripristino



Verifica il coinvolgimento del writer nelle sessioni di ripristino nel computer senza inviare eventi di **preripristino** o di **ripristino** a writer.

## <a name="syntax"></a>Sintassi

```
simulate restore
```

## <a name="remarks"></a>Note

-   **Simulate Restore** viene usato per verificare se il ripristino con Writer può essere eseguito correttamente.
-   Prima di poter usare **simulate Restore**, è necessario caricare un file di metadati DiskShadow usando il comando **Load Metadata** . Verranno caricati i writer e i componenti selezionati per il ripristino.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)