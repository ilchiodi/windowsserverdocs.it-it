---
title: extend
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2414e21d-fc0b-40e8-9e33-3e072f8ad76b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 11991f9fc338dca5201d8f9c9c598b9d7dcf239b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844784"
---
# <a name="extend"></a>extend

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

estende il volume o la partizione con lo stato attivo e la relativa file system in \(spazio libero\) non allocato su un disco.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
extend [size=<n>] [disk=<n>] [noerr]  
extend filesystem [noerr]  
```  
  
### <a name="parameters"></a>Parametri  
  
| Parametro  |                                                                                             Descrizione                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| dimensioni\=<n>  |      Specifica la quantità di spazio in megabyte \(MB\) da aggiungere alla partizione o volume corrente. Se si specifica alcuna dimensione, viene utilizzato tutto lo spazio libero contiguo disponibile sul disco.       |
| \=disco <n>  |                          Specifica il disco in cui viene esteso il volume o una partizione. Se non viene specificato alcun disco, viene esteso il volume o una partizione del disco corrente.                          |
| file System |                                   estende la file system del volume con lo stato attivo. Per utilizzare solo sui dischi in cui il file system non è stata estesa con il volume.                                    |
|   NOERR    | solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |
  
## <a name="remarks"></a>Note  
  
-   Nei dischi di base, lo spazio deve essere sullo stesso disco del volume o una partizione con lo stato attivo. Deve attenersi anche immediatamente il volume o una partizione con lo stato attivo \(ovvero deve iniziare in corrispondenza dell'offset del settore successivo\).  
  
-   Nei dischi dinamici con volumi semplici o con spanning, un volume può essere esteso nello spazio disponibile su qualsiasi disco dinamico. Questo comando, è possibile convertire un volume dinamico semplice in un volume dinamico con spanning. Il mirroring RAID\-5 o con striping non può essere estesa.  
  
-   Se la partizione è stata precedentemente formattata con il file system NTFS, il file system viene esteso automaticamente per riempire la partizione più grande e non si verificherà alcuna perdita di dati.  
  
-   Se la partizione è stata precedentemente formattata con un file system diverso da NTFS, il comando ha esito negativo senza apportare alcuna modifica alla partizione.  
  
-   Se la partizione non è stata formattata in precedenza con un file system, la partizione verrà comunque estesa.  
  
-   La partizione deve disporre di un volume associato prima può essere esteso.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Esempi  
Per estendere il volume o una partizione con lo stato attivo da 500 MB, sul disco 3, digitare:  
  
```  
extend size=500 disk=3  
```  
  
Per estendere il file system di un volume dopo che è stato esteso, digitare:  
  
```  
extend filesystem  
```  
  
## <a name="additional-references"></a>Altre informazioni di riferimento  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

