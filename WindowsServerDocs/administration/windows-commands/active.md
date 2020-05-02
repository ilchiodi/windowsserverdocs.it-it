---
title: active
description: Argomento di riferimento per il comando attivo, che su dischi di base, contrassegna la partizione con lo stato attivo come attivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 997c57b93434738c87396812c9b5e5b12d7a8e89
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719021"
---
# <a name="active"></a>active

Nei dischi di base Contrassegna partizione come attiva. Solo le partizioni possono essere contrassegnate come attive. Per eseguire questa operazione, è necessario selezionare una partizione. Usare il comando **select partition** per selezionare una partizione e spostare lo stato attivo su di essa.

> [!CAUTION]
> DiskPart informa solo il BIOS (Basic Input/Output System) o Extensible Firmware Interface (EFI) che la partizione o il volume è una partizione di sistema o un volume di sistema valido e che è in grado di contenere i file di avvio del sistema operativo. Ma non controlla il contenuto della partizione. Se l'errore si contrassegna una partizione come attiva e non contiene file di avvio del sistema operativo, il computer potrebbe non avviarsi.

## <a name="syntax"></a>Sintassi

```
active
```

## <a name="examples"></a>Esempi

Per contrassegnare la partizione come partizione attiva, digitare:

```
active
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando select partition](select-partition.md)
