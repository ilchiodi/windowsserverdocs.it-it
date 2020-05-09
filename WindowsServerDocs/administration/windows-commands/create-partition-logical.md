---
title: Crea partizione logica
description: Argomento di riferimento per il comando create partition logical, che consente di creare una partizione logica in una partizione estesa esistente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 99b8c837fe5da295087f9b146bf429d91ebc8693
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993272"
---
# <a name="create-partition-logical"></a>Crea partizione logica

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una partizione logica in una partizione estesa esistente. Dopo che la partizione è stata creata, lo stato attivo passa automaticamente alla nuova partizione.

>[!IMPORTANT]
> È possibile usare questo comando solo sui dischi MBR (master boot record). È necessario usare il comando [Seleziona disco](select-disk.md) per selezionare un disco MBR di base e spostare lo stato attivo a esso.
>
> Prima di poter creare unità logiche, è necessario creare una [partizione estesa](create-partition-extended.md) .

## <a name="syntax"></a>Sintassi

```
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| dimensioni =`<n>` | Specifica la dimensione della partizione logica in megabyte (MB), che deve essere minore della partizione estesa. Se si specifica alcuna dimensione, la partizione continua fino a quando non sia non è più disponibile spazio libero nella partizione estesa. |
| offset =`<n>` | Specifica l'offset in kilobyte (KB), in cui viene creata la partizione. L'offset Arrotonda per eccesso per riempire completamente tutte le dimensioni dei cilindri utilizzate. Se non viene specificato alcun offset, la partizione viene inserita nel primo extent del disco sufficientemente grande da contenerlo. La partizione è almeno la lunghezza in byte del numero specificato da **size =`<n>`**. Se si specifica una dimensione per la partizione logica, il valore deve essere minore della partizione estesa. |
| Allinea =`<n>` | Consente di allineare tutti gli extent volume o una partizione per il limite di allineamento più vicino. In genere utilizzata con le matrici numero RAID unità logica (LUN) hardware per migliorare le prestazioni. `<n>`numero di kilobyte (KB) dall'inizio del disco al limite di allineamento più vicino. |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

#### <a name="remarks"></a>Osservazioni

- Se non si specificano i parametri **size** e **offset** , la partizione logica viene creata nell'extent del disco più grande disponibile nella partizione estesa.

## <a name="examples"></a>Esempi

Per creare una partizione logica di 1000 megabyte di dimensioni, nella partizione estesa del disco selezionato digitare:

```
create partition logical size=1000
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Crea comando](create.md)

- [select disk](select-disk.md)
