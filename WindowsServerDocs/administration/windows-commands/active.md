---
title: active
description: Argomento i comandi di Windows per **active** - nei dischi di base Contrassegna partizione come attiva.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3a039e0200fb84d446739ac7017556b6c302f4af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868762"
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
-   Per eseguire questa operazione, è necessario selezionare una partizione. Usare la **Seleziona partizione** comando per selezionare una partizione e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per contrassegnare la partizione come partizione attiva, digitare:
```
active
```

#### <a name="additional-references"></a>Altri riferimenti

