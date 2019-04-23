---
title: creare una partizione efi
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3714cfe52aafd4a602346139552b6712dbbc98c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878222"
---
# <a name="create-partition-efi"></a>creare una partizione efi

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Nei processori Itanium\-i computer basati su, crea un'interfaccia Firmware estendibile \(EFI\) partizione di sistema in una tabella di partizione GUID \(gpt\) disco.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
create partition efi [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|size\=<n>|Le dimensioni della partizione in megabyte \(MB\). Se si specifica alcuna dimensione, la partizione continua fino a quando non sia disponibile spazio libero non è più nell'area corrente.|  
|offset\=<n>|L'offset in kilobyte \(KB\), in cui viene creata la partizione. Se l'offset non è specificato, la partizione viene inserita nel primo extent del disco di dimensioni sufficienti.|  
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|  
  
## <a name="remarks"></a>Note  
  
-   Dopo che la partizione è stata creata, lo stato attivo viene assegnato alla nuova partizione.  
  
-   Per eseguire questa operazione, è necessario selezionare un disco gpt. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.  
  
## <a name="BKMK_examples"></a>Esempi  
Per creare una partizione EFI di 1000 MB sul disco selezionato, digitare:  
  
```  
create partition efi size=1000  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  

  

