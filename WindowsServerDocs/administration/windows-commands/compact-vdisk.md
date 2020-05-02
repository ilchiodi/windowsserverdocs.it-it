---
title: compatta vdisk
description: Argomento di riferimento per il comando Compact vdisk, che consente di ridurre le dimensioni fisiche di un file di disco rigido virtuale (VHD) ad espansione dinamica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4ae5c653645c9f6f3ef97501a59932682c24be3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82711144"
---
# <a name="compact-vdisk"></a>compatta vdisk

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Riduce le dimensioni fisiche di un file di disco rigido virtuale (VHD) ad espansione dinamica. Questo parametro è utile perché i dischi rigidi virtuali aumento delle dimensioni a espansione dinamica è possibile aggiungere file, ma non automaticamente riducono le dimensioni quando si eliminano file.

## <a name="syntax"></a>Sintassi

```
compact vdisk
```

### <a name="remarks"></a>Osservazioni

- Per eseguire questa operazione, è necessario selezionare un disco rigido Virtuale ad espansione dinamica. Usare il [comando select vdisk](select-vdisk.md) per selezionare un disco rigido virtuale e spostare lo stato attivo su di esso.

- È possibile utilizzare solo dischi rigidi virtuali a espansione dinamica compatta scollegati o collegati come di sola lettura.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Connetti vdisk](attach-vdisk.md)

- [comando vdisk dettaglio](detail-vdisk.md)

- [Comando Scollega vdisk](detach-vdisk.md)

- [Espandi comando vdisk](expand-vdisk.md)

- [Comando merge vdisk](merge-vdisk.md)

- [Selezionare il comando vdisk](select-vdisk.md)

- [comando list](list.md)
