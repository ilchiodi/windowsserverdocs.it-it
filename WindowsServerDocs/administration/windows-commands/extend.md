---
title: extend
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2414e21d-fc0b-40e8-9e33-3e072f8ad76b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fdf070a733392d89bafe5bed5a1bf23d8e24d57
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439348"
---
# <a name="extend"></a>extend

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

estende il volume o una partizione con lo stato attivo e il relativo file system in gratuita \(unallocated\) spazio su disco.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
extend [size=<n>] [disk=<n>] [noerr]  
extend filesystem [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
| Parametro  |                                                                                             Descrizione                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| size\=<n>  |      Specifica la quantità di spazio in megabyte \(MB\) da aggiungere alla partizione o volume corrente. Se si specifica alcuna dimensione, viene utilizzato tutto lo spazio libero contiguo disponibile sul disco.       |
| disk\=<n>  |                          Specifica il disco in cui viene esteso il volume o una partizione. Se non viene specificato alcun disco, viene esteso il volume o una partizione del disco corrente.                          |
| file System |                                   estende il file system del volume con lo stato attivo. Per utilizzare solo sui dischi in cui il file system non è stata estesa con il volume.                                    |
|   NOERR    | Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |
  
## <a name="remarks"></a>Note  
  
-   Nei dischi di base, lo spazio deve essere sullo stesso disco del volume o una partizione con lo stato attivo. Deve attenersi anche immediatamente il volume o una partizione con lo stato attivo \(ovvero deve iniziare in corrispondenza dell'offset del settore successivo\).  
  
-   Nei dischi dinamici con volumi semplici o con spanning, un volume può essere esteso nello spazio disponibile su qualsiasi disco dinamico. Questo comando, è possibile convertire un volume dinamico semplice in un volume dinamico con spanning. Il mirroring RAID\-5 o con striping non può essere estesa.  
  
-   Se la partizione è stata precedentemente formattata con il file system NTFS, il file system viene esteso automaticamente fino a riempire la partizione più grande e si verificherà alcuna perdita di dati.  
  
-   Se la partizione è stata precedentemente formattata con un file system diversi da NTFS, il comando ha esito negativo senza alcuna modifica alla partizione.  
  
-   Se la partizione non è stata precedentemente formattata con un file system, la partizione ancora essere estesa.  
  
-   La partizione deve disporre di un volume associato prima può essere esteso.  
  
## <a name="BKMK_examples"></a>Esempi  
Per estendere il volume o una partizione con lo stato attivo da 500 MB, sul disco 3, digitare:  
  
```  
extend size=500 disk=3  
```  
  
Per estendere il file system di un volume dopo che è stato esteso, digitare:  
  
```  
extend filesystem  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

