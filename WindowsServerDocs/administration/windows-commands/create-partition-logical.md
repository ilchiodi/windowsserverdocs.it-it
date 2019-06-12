---
title: creare partizioni logiche
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d3af60aed6c8305e410c6ebfba3cf2e006034ad7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434156"
---
# <a name="create-partition-logical"></a>creare partizioni logiche

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una partizione logica in una partizione estesa esistente. È possibile usare questo comando solo sul record di avvio principale \(MBR\) dischi.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
|  Parametro  |                                                                                                                                                                                                                       Descrizione                                                                                                                                                                                                                        |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size\=<n>  |                                                                                                              Specifica le dimensioni della partizione logica espressa in megabyte \(MB\), che deve essere minore di quella della partizione estesa. Se si specifica alcuna dimensione, la partizione continua fino a quando non sia non è più disponibile spazio libero nella partizione estesa.                                                                                                               |
| offset\=<n> | Specifica l'offset in kilobyte \(KB\), in cui viene creata la partizione. La differenza di arrotondamento per eccesso a occupare qualsiasi dimensione cilindro viene utilizzata. Se non viene fornito alcun offset, la partizione viene inserita nel primo extent del disco sufficientemente grande per contenerlo. La partizione è grande almeno quanto in byte del numero specificato da **dimensioni\=<n>** . Se si specifica una dimensione per la partizione logica, deve essere minore di partizione estesa. |
| align\=<n>  |                                                                                     Consente di allineare tutti gli extent volume o una partizione per il limite di allineamento più vicino. In genere utilizzata con il numero di unità logica RAID hardware \(LUN\) matrici per migliorare le prestazioni.  <n> è il numero di kilobyte \(KB\) dall'inizio del disco per il limite di allineamento più vicino.                                                                                      |
|    NOERR    |                                                                                                                           Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.                                                                                                                           |
  
## <a name="remarks"></a>Note  
  
-   Se il **dimensioni** e **offset** parametri vengono omessi, viene creata la partizione logica dell'extent del disco più grande disponibile nella partizione estesa.  
  
-   Dopo aver creata la partizione, lo stato attivo passa automaticamente alla nuova partizione logica.  
  
-   Per eseguire questa operazione, è necessario selezionare un disco MBR di base. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.  
  
## <a name="BKMK_examples"></a>Esempi  
Per creare una partizione logica di 1000 MB di dimensioni, nella partizione estesa del disco selezionato, digitare:  
  
```  
create partition logical size=1000  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

