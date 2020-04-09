---
title: attivo
description: Windows Commands Topic for **Active**, che nei dischi di base, contrassegna la partizione con lo stato attivo come attivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42f2e0d367344355e8f9a570f37cfbdc5dfc4590
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851374"
---
# <a name="active"></a>attivo

Nei dischi di base Contrassegna partizione come attiva.

> [!CAUTION]
> DiskPart verifica che la partizione è in grado di contenere i file di avvio del sistema operativo. Ma non controlla il contenuto della partizione. Se l'errore si contrassegna una partizione come attiva e non contiene file di avvio del sistema operativo, il computer potrebbe non avviarsi.

## <a name="syntax"></a>Sintassi

```
active
```- 

## Remarks

-   This informs the basic input/output system (BIOS) or Extensible Firmware Interface (EFI) that the partition or volume is a valid system partition or system volume.

-   Only partitions can be marked as active.

-   A partition must be selected for this operation to succeed. Use the **select partition** command to select a partition and shift the focus to it.

## <a name=BKMK_examples></a>Examples

To mark the partition with focus as the active partition, type:

```
attivo
```
## Additional References

- [Command-Line Syntax Key](command-line-syntax-key.md)