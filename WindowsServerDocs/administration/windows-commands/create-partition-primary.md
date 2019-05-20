---
title: create partition primary
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d652d8e-3935-4a91-8ced-b17c0e7937be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbbb4b89540b7264fe907f79ca003117429da08e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881712"
---
# <a name="create-partition-primary"></a>create partition primary

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una partizione primaria del disco di base con lo stato attivo.  
  
> [!CAUTION]  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
create partition primary [size=<n>] [offset=<n>] [id={ <byte> | <guid> }] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|size\=<n>|Specifica la dimensione della partizione in megabyte \(MB\). Se si specifica alcuna dimensione, la partizione continua fino a quando è disponibile spazio nell'area corrente.|  
|offset\=<n>|L'offset in kilobyte \(KB\), in cui viene creata la partizione. Se l'offset non è specificato, la partizione inizierà all'inizio dell'extent del disco più grande di dimensioni sufficienti.|  
|align\=<n>|Consente di allineare tutti gli extent di partizione per il limite di allineamento più vicino. In genere utilizzata con il numero di unità logica RAID hardware \(LUN\) matrici per migliorare le prestazioni. <n> è il numero di kilobyte \(KB\) dall'inizio del disco per il limite di allineamento più vicino.|  
|ID\={ <byte> &#124; <guid> }|Specifica il tipo di partizione. Questo parametro è destinato all'OEM \(OEM\) utilizzare solo. Con questo parametro è possibile specificare qualsiasi tipo di partizione o GUID. Ma non controlla il tipo di partizione per la validità tranne assicurarsi che sia un byte in formato esadecimale o un GUID. **Attenzione:** Creazione di partizioni con questo parametro potrebbe avere esito negativo o non essere possibile per l'avvio del computer. A meno che non si è un OEM o un professionista IT esperto dischi gpt, non creare partizioni in dischi gpt utilizzando questo parametro. Utilizzare sempre il **creare una partizione efi** comando per creare partizioni di sistema EFI, il **creare partizione msr** comando per creare partizioni Microsoft Reserved e il **creare partizione primaria** comandi \(senza il **id\={byte&#124;guid}** parametro\) per creare partizioni primarie nei dischi gpt.<br /><br />**Dischi record di avvio principale**<br /><br />per record di avvio principale \(MBR\) dischi, si specifica un tipo di partizione, in formato esadecimale, per la partizione. Se questo parametro non è specificato per un disco MBR, il comando crea una partizione di tipo 0x06, che specifica che un file system non è installato. Ecco alcuni esempi:<br /><br />-Partizione dati LDM: 0x42<br />-partizione di ripristino: 0x27<br />-Riconosciuto partizione OEM: 0x12, 0x84, 0xDE, 0xFE, 0xA0<br /><br />**Dischi tabella di partizione GUID**<br /><br />per la tabella di partizione GUID \(gpt\) dischi, è possibile specificare un tipo di partizione GUID per la partizione che si desidera creare. GUID riconosciuto includono:<br /><br />-Partizione di sistema EFI: c12a7328\-f81f\-11d 2\-ba4b\-00a0c93ec93b<br />-Partizione Microsoft Reserved: e3c9e316\-0b5c\-4db8\-d 817\-f92df00215ae<br />-Partizione di dati di base: ebd0a0a2\-b9e5\-4433\-c 87 0\-68b6b72699c7<br />-Partizione metadati LDM su un disco dinamico: 5808c8aa\-7e8f\-42e0\-85d2\-e1e90434cfb3<br />-Partizione di dati Gestione dischi LOGICI su un disco dinamico: af9b60a0\-1431\-4f62\-bc68\-3311714a69ad<br />-partizione di ripristino: de94bba4\-d 06 1\-4D 40\-a16a\-bfd50179d6ac<br /><br />Se questo parametro viene omesso per un disco gpt, il comando crea una partizione di dati di base.|  
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza il parametro noerr, un errore causa DiskPart viene interrotto con un codice di errore.|  
  
## <a name="remarks"></a>Note  
  
-   Dopo aver creato la partizione, lo stato attivo passa automaticamente alla nuova partizione.  
  
-   La partizione non riceve una lettera di unità. È necessario utilizzare il **assegnare** comando DiskPart per assegnare una lettera di unità alla partizione.  
  
-   Per eseguire questa operazione, è necessario selezionare un disco di base. Usare la **disco selezionare** comando per selezionare un disco di base e spostare lo stato attivo a esso.  
  
## <a name="BKMK_examples"></a>Esempi  
Per creare una partizione primaria di 1000 MB di dimensioni, digitare:  
  
```  
create partition primary size=1000  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  

  

