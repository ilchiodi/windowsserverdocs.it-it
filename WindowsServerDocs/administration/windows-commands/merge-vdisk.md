---
title: Unisci vdisk
description: Argomento di riferimento per il comando merge vdisk, che unisce un disco rigido virtuale differenze (VHD) con il VHD padre corrispondente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5865bb08-89a3-406c-8328-0ef8868d03e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ccd288baff691576c15c3e9c686b6708d1c45ee8
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354621"
---
# <a name="merge-vdisk"></a>Unisci vdisk

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Unisce un disco rigido virtuale differenze (VHD) con il VHD padre corrispondente. Il VHD padre verrà modificato in modo da includere le modifiche dal disco rigido virtuale differenze. Questo comando modifica il VHD padre. Di conseguenza, altri dischi rigidi virtuali differenze che dipendono dall'elemento padre non saranno più validi.

> [!IMPORTANT]
> Per eseguire questa operazione, è necessario scegliere e scollegare un disco rigido virtuale. Utilizzare il **Selezionare vdisk** comando per selezionare un disco rigido Virtuale e spostare lo stato attivo a esso.

## <a name="syntax"></a>Sintassi

```
merge vdisk depth=<n>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| profondità =`<n>` | Indica il numero di file VHD padre da unire insieme. Ad esempio, `depth=1` indica che il disco rigido virtuale differenze verrà unito a un livello della catena di differenziazione. |

### <a name="examples"></a>Esempio

Per unire un disco rigido virtuale differenze con il VHD padre, digitare:

```
merge vdisk depth=1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Connetti vdisk](attach-vdisk.md)

- [comando Compact vdisk](compact-vdisk.md)

- [comando vdisk dettaglio](detail-vdisk.md)

- [comando Scollega vdisk](detach-vdisk.md)

- [Espandi comando vdisk](expand-vdisk.md)

- [Selezionare il comando vdisk](select-vdisk.md)

- [comando list](list.md)
