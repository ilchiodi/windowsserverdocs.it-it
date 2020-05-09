---
title: Crea volume semplice
description: Argomento di riferimento per il comando Crea volume semplice, che consente di creare un volume semplice sul disco dinamico specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eb5aeebbcbde581fe3259d6cb5aca6445a3b27aa
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993216"
---
# <a name="create-volume-simple"></a>Crea volume semplice

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un volume semplice sul disco dinamico specificato. Dopo aver creato il volume, lo stato attivo passa automaticamente al nuovo volume.

## <a name="syntax"></a>Sintassi

```
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| dimensioni =`<n>`  | Dimensioni del volume in megabyte (MB). Se si specifica alcuna dimensione, il nuovo volume occuperà lo spazio rimanente sul disco. |
| disco =`<n>`  | Disco dinamico in cui viene creato il volume. Se non viene specificato alcun disco, viene utilizzato il disco corrente. |
| Allinea =`<n>` | Consente di allineare tutti gli extent di volume per il limite di allineamento più vicino. In genere utilizzata con le matrici numero RAID unità logica (LUN) hardware per migliorare le prestazioni. `<n>`numero di kilobyte (KB) dall'inizio del disco al limite di allineamento più vicino. |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="examples"></a>Esempi

Per creare un volume di 1000 MB, le dimensioni su disco 1, digitare:

```
create volume simple size=1000 disk=1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Crea comando](create.md)
