---
title: compatta vdisk
description: I comandi di Windows argomento forcompact vdisk, che consente di ridurre le dimensioni fisiche di un file di disco rigido virtuale (VHD) ad espansione dinamica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9691be21c188fbc2c3b2e782acde127270decf56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847424"
---
# <a name="compact-vdisk"></a>compatta vdisk

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Riduce le dimensioni fisiche di un file di disco rigido virtuale (VHD) ad espansione dinamica. Questo parametro è utile perché i dischi rigidi virtuali aumento delle dimensioni a espansione dinamica è possibile aggiungere file, ma non automaticamente riducono le dimensioni quando si eliminano file.

> [!NOTE]
> Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.

## <a name="syntax"></a>Sintassi
```
compact vdisk
```

## <a name="remarks"></a>Note

- Per eseguire questa operazione, è necessario selezionare un disco rigido Virtuale ad espansione dinamica. Utilizzare il **Selezionare vdisk** comando per selezionare un disco rigido Virtuale e spostare lo stato attivo a esso.

- È solo possibile compattare i dischi rigidi virtuali che vengono scollegati o collegati in sola lettura a espansione dinamica.

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi
Per compattare un disco rigido Virtuale ad espansione dinamica, digitare:
```
compact vdisk
```

## <a name="additional-references"></a>Altre informazioni di riferimento
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
- [Connetti vdisk](attach-vdisk.md)
- [Dettagli vdisk](detail-vdisk.md)
- [Scollega vdisk](detach-vdisk.md)
- [Espandi vdisk](expand-vdisk.md)
- [Unisci vdisk](merge-vdisk.md)
- [Seleziona vdisk](select-vdisk.md)
- [list_1](list_1.md)
