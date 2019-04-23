---
title: creare partizione msr
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 79288fe90d037659f5e3934f1925dd8b7c21ad7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873432"
---
# <a name="create-partition-msr"></a>creare partizione msr

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un Reserved Microsoft \(MSR\) partizione in una tabella di partizione GUID \(gpt\) disco.  
  
> [!CAUTION]  
> Prestare molta attenzione quando si utilizza questo comando. Poiché i dischi gpt richiedono un layout di partizione specifica, creare partizioni Microsoft Reserved potrebbe causare il disco illeggibile.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
create partition msr [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|size\=<n>|Le dimensioni della partizione in megabyte \(MB\). La partizione è grande almeno quanto in byte del numero specificato da <n>. Se si specifica alcuna dimensione, la partizione continua fino a quando non sia disponibile spazio libero non è più nell'area corrente.|  
|offset\=<n>|Specifica l'offset in kilobyte \(KB\), in cui viene creata la partizione. La differenza di arrotondamento per eccesso a occupare qualsiasi dimensione di settore viene utilizzata. Se l'offset non è specificato, la partizione viene inserita nel primo extent del disco di dimensioni sufficienti.|  
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|  
  
## <a name="remarks"></a>Note  
  
-   Nei dischi gpt che vengono utilizzati per avviare il sistema operativo Windows, Extensible Firmware Interface \(EFI\) partizione di sistema è la prima partizione del disco, seguita dalla partizione Reserved Microsoft. i dischi GPT utilizzati solo per l'archiviazione dei dati non è una partizione di sistema EFI, nel qual caso la partizione Reserved Microsoft è la prima partizione.  
  
-   Windows non viene installato Microsoft Reserved partizioni. È possibile archiviare i dati su di essi e non eliminarli.  
  
-   In ogni disco gpt è necessaria una partizione Reserved Microsoft. Le dimensioni di questa partizione dipendono dalle dimensioni totali del disco gpt. Le dimensioni del disco gpt devono essere almeno 32 MB per creare una partizione Reserved Microsoft.  
  
-   Per eseguire questa operazione, è necessario selezionare un disco gpt di base. Usare la **disco selezionare** comando per selezionare un disco gpt di base e spostare lo stato attivo a esso.  
  
## <a name="BKMK_examples"></a>Esempi  
Per creare una partizione di Microsoft Reserved di 1000 MB di dimensioni, digitare:  
  
```  
create partition msr size=1000  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  

  

