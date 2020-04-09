---
title: Simula ripristino
description: Windows Commands argomento for simulate Restore, che verifica il coinvolgimento del writer nelle sessioni di ripristino nel computer senza inviare eventi di preripristino o di ripristino postripristino ai writer.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 024d654864c000e44bccb9ddb167c6147444cc00
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834104"
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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)