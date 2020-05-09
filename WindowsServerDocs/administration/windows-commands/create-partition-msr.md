---
title: Crea partizione MSR
description: Argomento di riferimento per create partition MSR, che consente di creare una partizione Microsoft riservata (MSR) in un disco GPT (GUID Partition Table).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 04fba033-23cb-4521-bd5d-db96131f2e73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d427e9f96023f8b66f72e3895b30519ab7cd2de1
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993256"
---
# <a name="create-partition-msr"></a>Crea partizione MSR

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una partizione Microsoft riservata (MSR) in un disco della tabella di partizione GUID (GPT). In ogni disco GPT è richiesta una partizione riservata Microsoft. Le dimensioni di questa partizione dipendono dalla dimensione totale del disco GPT. Per creare una partizione riservata Microsoft, è necessario che le dimensioni del disco GPT siano di almeno 32 MB.

> [!IMPORTANT]
> Prestare molta attenzione quando si utilizza questo comando. Poiché i dischi GPT richiedono un layout di partizione specifico, la creazione di partizioni riservate Microsoft può rendere illeggibile il disco.
>
> Per eseguire questa operazione, è necessario selezionare un disco GPT di base. È necessario utilizzare il comando [Seleziona disco](select-disk.md) per selezionare un disco GPT di base e spostare lo stato attivo a esso.

## <a name="syntax"></a>Sintassi

```
create partition msr [size=<n>] [offset=<n>] [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| dimensioni =`<n>` | Dimensioni della partizione in megabyte (MB). La partizione è grande almeno quanto in byte del numero specificato da `<n>`. Se si specifica alcuna dimensione, la partizione continua fino a quando non sia disponibile spazio libero non è più nell'area corrente. |
| offset =`<n>` | Specifica l'offset in kilobyte (KB), in cui viene creata la partizione. La differenza di arrotondamento per eccesso a occupare qualsiasi dimensione di settore viene utilizzata. Se l'offset non è specificato, la partizione viene inserita nel primo extent del disco di dimensioni sufficienti. |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

#### <a name="remarks"></a>Osservazioni

- Nei dischi GPT usati per avviare il sistema operativo Windows, la partizione di sistema Extensible Firmware Interface (EFI) è la prima partizione del disco, seguita dalla partizione riservata Microsoft. i dischi GPT usati solo per l'archiviazione dei dati non hanno una partizione di sistema EFI, nel qual caso la partizione riservata Microsoft è la prima partizione.

- Windows non monta le partizioni riservate Microsoft. È possibile archiviare i dati su di essi e non eliminarli.

## <a name="examples"></a>Esempi

Per creare una partizione di Microsoft Reserved di 1000 MB di dimensioni, digitare:

```
create partition msr size=1000
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Crea comando](create.md)

- [select disk](select-disk.md)
