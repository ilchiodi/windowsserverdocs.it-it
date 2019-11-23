---
title: Crea partizione estesa
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4ad7cb66-9c66-4153-b94e-1030a7225070
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21620da46be0e1375f320172e7ccfe2edc338114
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378909"
---
# <a name="create-partition-extended"></a>Crea partizione estesa

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una partizione estesa sul disco con lo stato attivo. È possibile utilizzare questo comando solo sul Master Boot Record \(MBR\) dischi.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
|  Parametro  |                                                                                                                             Descrizione                                                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  dimensioni\=<n>  |                                                  Specifica la dimensione della partizione in megabyte \(MB\). Se si specifica alcuna dimensione, la partizione continua fino a quando non sia non è più disponibile spazio libero nella partizione estesa.                                                  |
| offset\=<n> |                     Specifica l'offset in kilobyte \(KB\), in cui viene creata la partizione. Se l'offset non è specificato, la partizione inizierà all'inizio dello spazio libero sul disco sia sufficiente per contenere la nuova partizione.                      |
| Allinea\=<n>  | Consente di allineare tutti gli extent di partizione per il limite di allineamento più vicino. In genere utilizzata con il numero di unità logica RAID hardware \(LUN\) matrici per migliorare le prestazioni. <n> è il numero di kilobyte \(KB\) dall'inizio del disco al limite di allineamento più vicino. |
|    NOERR    |                                 solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.                                 |
  
## <a name="remarks"></a>Osservazioni  
  
-   Dopo che la partizione è stata creata, lo stato attivo passa automaticamente alla nuova partizione.  
  
-   Può includere una sola partizione per ogni disco.  
  
-   Questo comando non riesce se si tenta di creare una partizione estesa all'interno di un'altra partizione estesa.  
  
-   È necessario creare una partizione estesa prima di poter creare unità logiche.  
  
-   Per eseguire questa operazione, è necessario selezionare un disco MBR di base. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.  
  
## <a name="BKMK_examples"></a>Esempi  
Per creare una partizione estesa di 1000 MB di dimensioni, digitare:  
  
```  
create partition extended size=1000  
```  
  
#### <a name="additional-references"></a>riferimenti aggiuntivi  
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

