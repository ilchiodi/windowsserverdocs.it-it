---
title: Crea partizione logica
description: Argomento di riferimento per create partition logical, che consente di creare una partizione logica in una partizione estesa esistente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43d1c26d785fcecbb81a99c7484a15b3ea8b58a2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719241"
---
# <a name="create-partition-logical"></a>Crea partizione logica

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una partizione logica in una partizione estesa esistente. È possibile usare questo comando solo nei dischi MBR (master boot record).

## <a name="syntax"></a>Sintassi  
  
```  
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parametri  
  
|  Parametro  |                                                                                                                                                                                                                       Descrizione                                                                                                                                                                                                                        |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  dimensioni\=<n>  |                                                                                                              Specifica la dimensione della partizione logica in megabyte \(MB\), che deve essere minore della partizione estesa. Se si specifica alcuna dimensione, la partizione continua fino a quando non sia non è più disponibile spazio libero nella partizione estesa.                                                                                                               |
| offset\=<n> | Specifica l'offset in kilobyte \(KB\), in cui viene creata la partizione. L'offset Arrotonda per eccesso per riempire completamente tutte le dimensioni dei cilindri utilizzate. Se non viene specificato alcun offset, la partizione viene inserita nel primo extent del disco sufficientemente grande da contenerlo. La partizione è almeno la lunghezza in byte del numero specificato dalle **dimensioni\=**. Se si specifica una dimensione per la partizione logica, il valore deve essere minore della partizione estesa. |
| allineare\=<n>  |                                                                                     Consente di allineare tutti gli extent volume o una partizione per il limite di allineamento più vicino. In genere utilizzata con il numero di unità logica RAID hardware \(LUN\) matrici per migliorare le prestazioni.  <n>indica il numero di kilobyte \(KB\) dall'inizio del disco al limite di allineamento più vicino.                                                                                      |
|    NOERR    |                                                                                                                           solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.                                                                                                                           |
  
## <a name="remarks"></a>Osservazioni  
  
-   Se i parametri **size** e **offset** non vengono specificati, la partizione logica viene creata nell'extent del disco più grande disponibile nella partizione estesa.  
  
-   Dopo la creazione della partizione, lo stato attivo passa automaticamente alla nuova partizione logica.  
  
-   Per eseguire questa operazione, è necessario selezionare un disco MBR di base. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.  
  
## <a name="examples"></a>Esempi  
Per creare una partizione logica di 1000 megabyte di dimensioni, nella partizione estesa del disco selezionato digitare:  
  
```  
create partition logical size=1000  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

