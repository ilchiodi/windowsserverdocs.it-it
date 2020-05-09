---
title: Crea partizione estesa
description: Argomento di riferimento per il comando di creazione partizione estesa, che consente di creare una partizione estesa sul disco con lo stato attivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4ad7cb66-9c66-4153-b94e-1030a7225070
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d2cc591dca276f70b3ddda1827607d5e2953ed2
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993276"
---
# <a name="create-partition-extended"></a>Crea partizione estesa

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una partizione estesa sul disco con lo stato attivo. Dopo che la partizione è stata creata, lo stato attivo passa automaticamente alla nuova partizione.

>[!IMPORTANT]
> È possibile usare questo comando solo sui dischi MBR (master boot record). È necessario usare il comando [Seleziona disco](select-disk.md) per selezionare un disco MBR di base e spostare lo stato attivo a esso.
>
> È necessario creare una partizione estesa prima di poter creare unità logiche. Può includere una sola partizione per ogni disco. Questo comando non riesce se si tenta di creare una partizione estesa all'interno di un'altra partizione estesa.

## <a name="syntax"></a>Sintassi

```
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| dimensioni =`<n>` | Specifica le dimensioni della partizione in megabyte (MB). Se si specifica alcuna dimensione, la partizione continua fino a quando non sia non è più disponibile spazio libero nella partizione estesa. |
| offset =`<n>` | Specifica l'offset in kilobyte (KB), in cui viene creata la partizione. Se l'offset non è specificato, la partizione inizierà all'inizio dello spazio libero sul disco sia sufficiente per contenere la nuova partizione. |
| Allinea =`<n>` | Consente di allineare tutti gli extent di partizione per il limite di allineamento più vicino. In genere utilizzata con le matrici numero RAID unità logica (LUN) hardware per migliorare le prestazioni. `<n>`numero di kilobyte (KB) dall'inizio del disco al limite di allineamento più vicino. |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="examples"></a>Esempi

Per creare una partizione estesa di 1000 MB di dimensioni, digitare:

```
create partition extended size=1000
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Crea comando](create.md)

- [select disk](select-disk.md)
