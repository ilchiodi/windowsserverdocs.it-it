---
title: Crea partizione logica
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f18048272eda710f7cb53a631ddeda81784a56b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378887"
---
# <a name="create-partition-logical"></a>Crea partizione logica

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una partizione logica in una partizione estesa esistente. È possibile usare questo comando solo nel record di avvio principale \(MBR @ no__t-1 Disks.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
|  Parametro  |                                                                                                                                                                                                                       Descrizione                                                                                                                                                                                                                        |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  dimensioni @ no__t-0 @ no__t-1  |                                                                                                              Specifica la dimensione della partizione logica in megabyte \(MB @ no__t-1, che deve essere minore della partizione estesa. Se si specifica alcuna dimensione, la partizione continua fino a quando non sia non è più disponibile spazio libero nella partizione estesa.                                                                                                               |
| offset\=<n> | Specifica l'offset in kilobyte \(KB\), in cui viene creata la partizione. L'offset Arrotonda per eccesso per riempire completamente tutte le dimensioni dei cilindri utilizzate. Se non viene specificato alcun offset, la partizione viene inserita nel primo extent del disco sufficientemente grande da contenerlo. La partizione è almeno la lunghezza in byte del numero specificato dalle **dimensioni @ no__t-1 @ no__t-2**. Se si specifica una dimensione per la partizione logica, il valore deve essere minore della partizione estesa. |
| align @ no__t-0 @ no__t-1  |                                                                                     Consente di allineare tutti gli extent volume o una partizione per il limite di allineamento più vicino. In genere utilizzata con il numero di unità logica RAID hardware \(LUN\) matrici per migliorare le prestazioni.  <n> indica il numero di kilobyte \( KB @ no__t-2 dall'inizio del disco al limite di allineamento più vicino.                                                                                      |
|    NOERR    |                                                                                                                           Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.                                                                                                                           |
  
## <a name="remarks"></a>Note  
  
-   Se i parametri **size** e **offset** non vengono specificati, la partizione logica viene creata nell'extent del disco più grande disponibile nella partizione estesa.  
  
-   Dopo la creazione della partizione, lo stato attivo passa automaticamente alla nuova partizione logica.  
  
-   Per eseguire questa operazione, è necessario selezionare un disco MBR di base. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.  
  
## <a name="BKMK_examples"></a>Esempi  
Per creare una partizione logica di 1000 megabyte di dimensioni, nella partizione estesa del disco selezionato digitare:  
  
```  
create partition logical size=1000  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

