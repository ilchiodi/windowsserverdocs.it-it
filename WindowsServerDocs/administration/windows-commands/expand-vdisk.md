---
title: Espandi vdisk
description: Argomento di riferimento per il comando Expand vdisk, che espande un disco rigido virtuale (VHD) a una dimensione specificata.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 48973178f35f792b52fa81e5ed59449ca5db2559
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436186"
---
# <a name="expand-vdisk"></a>Espandi vdisk

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Espande un disco rigido virtuale (VHD) a una dimensione specificata.

Un disco rigido Virtuale deve essere selezionato e scollegato per eseguire questa operazione. Usare il [comando select vdisk](select-vdisk.md) per selezionare un volume e spostare lo stato attivo a esso.

## <a name="syntax"></a>Sintassi

```
expand vdisk maximum=<n>
```

### <a name="parameters"></a>Parametri

 | Parametro | Descrizione |
 |---------- | ----------- |
 | massimo =`<n>` | Specifica la nuova dimensione per il disco rigido Virtuale in megabyte (MB). |

### <a name="examples"></a>Esempi

Per espandere il disco rigido Virtuale selezionato 20 GB, digitare:

```
expand vdisk maximum=20000
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Selezionare il comando vdisk](select-vdisk.md)

- [comando Connetti vdisk](attach-vdisk.md)

- [comando Compact vdisk](compact-vdisk.md)

- [comando Scollega vdisk](detach-vdisk.md)

- [comando vdisk dettaglio](detail-vdisk.md)

- [comando merge vdisk](merge-vdisk.md)

- [comando list](list.md)
