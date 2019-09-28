---
title: Crea partizione MSR
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04fba033-23cb-4521-bd5d-db96131f2e73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45cc215b097ce048b15f0e907f95f976e4941e28
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378904"
---
# <a name="create-partition-msr"></a>Crea partizione MSR

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una partizione Microsoft riservata \(MSR @ no__t-1 in una tabella di partizione GUID \(gpt @ no__t-3 disco.  
  
> [!CAUTION]  
> Prestare molta attenzione quando si utilizza questo comando. Poiché i dischi GPT richiedono un layout di partizione specifico, la creazione di partizioni riservate Microsoft potrebbe rendere illeggibile il disco.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
create partition msr [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
|  Parametro  |                                                                                                                         Descrizione                                                                                                                         |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  dimensioni @ no__t-0 @ no__t-1  |               Le dimensioni della partizione in megabyte \(MB\). La partizione è grande almeno quanto in byte del numero specificato da <n>. Se si specifica alcuna dimensione, la partizione continua fino a quando non sia disponibile spazio libero non è più nell'area corrente.               |
| offset\=<n> | Specifica l'offset in kilobyte \(KB\), in cui viene creata la partizione. La differenza di arrotondamento per eccesso a occupare qualsiasi dimensione di settore viene utilizzata. Se l'offset non è specificato, la partizione viene inserita nel primo extent del disco di dimensioni sufficienti. |
|    NOERR    |                            Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.                             |
  
## <a name="remarks"></a>Note  
  
-   Nei dischi GPT usati per avviare il sistema operativo Windows, la partizione di sistema Extensible Firmware Interface \(EFI @ no__t-1 è la prima partizione del disco, seguita dalla partizione riservata Microsoft. i dischi GPT usati solo per l'archiviazione dei dati non hanno una partizione di sistema EFI, nel qual caso la partizione riservata Microsoft è la prima partizione.  
  
-   Windows non viene installato Microsoft Reserved partizioni. È possibile archiviare i dati su di essi e non eliminarli.  
  
-   In ogni disco GPT è richiesta una partizione riservata Microsoft. Le dimensioni di questa partizione dipendono dalla dimensione totale del disco GPT. Per creare una partizione riservata Microsoft, è necessario che le dimensioni del disco GPT siano di almeno 32 MB.  
  
-   Per eseguire questa operazione, è necessario selezionare un disco GPT di base. Usare il comando **Seleziona disco** per selezionare un disco GPT di base e spostare lo stato attivo a esso.  
  
## <a name="BKMK_examples"></a>Esempi  
Per creare una partizione di Microsoft Reserved di 1000 MB di dimensioni, digitare:  
  
```  
create partition msr size=1000  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

