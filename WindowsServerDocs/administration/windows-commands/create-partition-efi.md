---
title: creare una partizione EFI
description: Argomento di riferimento per create partition EFI, che consente di creare una partizione di sistema Extensible Firmware Interface (EFI) in un disco GPT (GUID Partition Table) in computer basati su Itanium.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cc505dcbd94cf616b4ea160501ed1eadc1456a18
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719265"
---
# <a name="create-partition-efi"></a>creare una partizione EFI

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Nei computer basati su Itanium crea una partizione di sistema Extensible Firmware Interface (EFI) in un disco GPT (GUID Partition Table).

## <a name="syntax"></a>Sintassi  
  
```  
create partition efi [size=<n>] [offset=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parametri  
  
|  Parametro  |                                                                                             Descrizione                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  dimensioni\=<n>  |                         Le dimensioni della partizione in megabyte \(MB\). Se si specifica alcuna dimensione, la partizione continua fino a quando non sia disponibile spazio libero non è più nell'area corrente.                         |
| offset\=<n> |             L'offset in kilobyte \(KB\), in cui viene creata la partizione. Se l'offset non è specificato, la partizione viene inserita nel primo extent del disco di dimensioni sufficienti.              |
|    NOERR    | solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |
  
## <a name="remarks"></a>Osservazioni  
  
-   Dopo che la partizione è stata creata, lo stato attivo viene assegnato alla nuova partizione.  
  
-   Per eseguire questa operazione, è necessario selezionare un disco GPT. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.  
  
## <a name="examples"></a>Esempi  
Per creare una partizione EFI di 1000 MB sul disco selezionato, digitare:  
  
```  
create partition efi size=1000  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

