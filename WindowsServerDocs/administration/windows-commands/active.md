---
title: active
description: Windows Commands Topic for **Active** -on Basic disks contrassegna la partizione con lo stato attivo come attivo.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c926bf9b7a583cf7eaa23166e09e6f0a1599e625
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382854"
---
# <a name="active"></a>active



Nei dischi di base Contrassegna partizione come attiva.

> [!CAUTION]
> DiskPart verifica che la partizione è in grado di contenere i file di avvio del sistema operativo. Ma non controlla il contenuto della partizione. Se l'errore si contrassegna una partizione come attiva e non contiene file di avvio del sistema operativo, il computer potrebbe non avviarsi.

## <a name="syntax"></a>Sintassi

```
active
```

## <a name="remarks"></a>Note

-   Informa il basic input/output system (BIOS) o l'interfaccia EFI (Extensible Firmware Interface) che la partizione o il volume è una partizione di sistema valida o un volume di sistema.
-   Solo le partizioni possono essere contrassegnate come attive.
-   Per eseguire questa operazione, è necessario selezionare una partizione. Usare il comando **select partition** per selezionare una partizione e spostare lo stato attivo su di essa.

## <a name="BKMK_examples"></a>Esempi

Per contrassegnare la partizione come partizione attiva, digitare:
```
active
```

#### <a name="additional-references"></a>Altri riferimenti

