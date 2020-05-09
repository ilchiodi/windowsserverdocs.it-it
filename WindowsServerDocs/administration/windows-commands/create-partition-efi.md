---
title: creare una partizione EFI
description: Argomento di riferimento per il comando create partition EFI, che consente di creare una partizione di sistema Extensible Firmware Interface (EFI) in un disco GPT (GUID Partition Table) in computer basati su Itanium.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8c314d756bebd0d0ec2ed9c844f714f395d04c6b
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993289"
---
# <a name="create-partition-efi"></a>creare una partizione EFI

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una partizione di sistema Extensible Firmware Interface (EFI) in un disco della tabella di partizione GUID (GPT) in computer basati su Itanium. Dopo la creazione della partizione, lo stato attivo viene assegnato alla nuova partizione.

>[!NOTE]
> Per eseguire questa operazione, è necessario selezionare un disco GPT. Utilizzare il [disco selezionare](select-disk.md) comando per selezionare un disco e spostare lo stato attivo a esso.

## <a name="syntax"></a>Sintassi

```
create partition efi [size=<n>] [offset=<n>] [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| dimensioni =`<n>` | Dimensioni della partizione in megabyte (MB). Se si specifica alcuna dimensione, la partizione continua fino a quando non sia disponibile spazio libero non è più nell'area corrente. |
| offset =`<n>` | Offset in kilobyte (KB), in cui viene creata la partizione. Se l'offset non è specificato, la partizione viene inserita nel primo extent del disco di dimensioni sufficienti. |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

#### <a name="remarks"></a>Osservazioni

- È necessario aggiungere almeno un volume con il **aggiungere volume** comando prima di poter utilizzare il **creare** comando.

- Dopo aver eseguito la **creare** è possibile utilizzare il comando di **exec** comando per eseguire uno script di duplicazione per il backup della copia shadow.

- È possibile utilizzare il **iniziare backup** dei comandi per specificare un backup completo, anziché un backup di copia.

## <a name="examples"></a>Esempi

Per creare una partizione EFI di 1000 MB sul disco selezionato, digitare:

```
create partition efi size=1000
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Crea comando](create.md)

- [select disk](select-disk.md)
