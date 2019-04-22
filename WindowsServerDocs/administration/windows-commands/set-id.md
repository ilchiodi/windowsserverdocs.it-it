---
title: ID del set
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5793d7ad-827e-4285-b2c6-ae60eeb0e886
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f95490850acd263fb0b34007ac64a84c9a374865
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822052"
---
# <a name="set-id"></a>ID del set

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il comando Diskpart imposta ID modifica il campo tipo di partizione per la partizione con lo stato attivo.  
  
> [!IMPORTANT]  
> Questo comando deve essere utilizzato per gli original equipment manufacturer \(OEM\) solo. Modifica di campi di tipo di partizione con questo parametro potrebbe non riuscire o non essere in grado di eseguire l'avvio del computer. A meno che non OEM o esperti in dischi gpt, non è necessario modificare i campi di tipo di partizione nei dischi gpt utilizzando questo parametro. Utilizzare sempre il [creare una partizione efi](create-partition-efi.md) comando per creare partizioni di sistema EFI, il [creare partizione msr](create-partition-msr.md) comando per creare partizioni Microsoft Reserved e il [creare partizione primaria](create-partition-primary.md) comando senza il parametro ID per creare partizioni primarie nei dischi gpt.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
set id={ <byte> | <GUID> } [override] [noerr]  
```  
  
## <a name="parameters"></a>Parametri  
  
|Parametro|Descrizione|  
|-------|--------|  
|<byte>|per record di avvio principale \(MBR\) dischi, specifica il nuovo valore per il campo di tipo, in formato esadecimale, per la partizione. Con questo parametro, ad eccezione di tipo 0x42, che specifica una partizione di gestione dischi LOGICI, è possibile specificare qualsiasi tipo di partizione. Si noti che il 0x iniziale viene omesso quando si specifica il tipo di partizione esadecimale.|  
|<GUID>|per la tabella di partizione GUID \(gpt\) dischi, specifica il nuovo valore GUID per il campo di tipo per la partizione. GUID riconosciuto includono:<br /><br />-Partizione di sistema EFI: c12a7328\-f81f\-11d 2\-ba4b\-00a0c93ec93b<br />-Partizione di dati di base: ebd0a0a2\-b9e5\-4433\-c 87 0\-68b6b72699c7<br /><br />Qualsiasi tipo di partizione GUID può essere specificato con questo parametro ad eccezione dei seguenti:<br /><br />-Partizione Microsoft Reserved: e3c9e316\-0b5c\-4db8\-d 817\-f92df00215ae<br />-Partizione metadati LDM su un disco dinamico: 5808c8aa\-7e8f\-42e0\-d 85 2\-e1e90434cfb3<br />-Partizione di dati Gestione dischi LOGICI su un disco dinamico: af9b60a0\-1431\-4f62\-bc68\-3311714a69ad<br />-Cluster partizione metadati: db97dba9\-0840\-4bae\-97f0\-ffb9a327c7e1|  
|eseguire l'override|forza il file system sul volume per smontare prima di modificare il tipo di partizione. Quando si esegue il **id Set** comando DiskPart tenta di bloccare e smontare il file system sul volume. Se **override** non è specificato, e la chiamata di bloccare il file system non riesce \(ad esempio, poiché non esiste un handle aperto\), l'operazione avrà esito negativo. Quando **sostituzione** viene specificato, DiskPart forza la disinstallazione anche se ha esito negativo della chiamata a bloccare il file system e tutti gli handle aperti per il volume non saranno più validi.<br /><br />Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.|  
|NOERR|Usato solo negli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|  
  
## <a name="remarks"></a>Note  
  
-   Oltre alle limitazioni menzionate in precedenza, ma non controlla la validità del valore specificato \(tranne assicurarsi che sia un byte in formato esadecimale o un GUID\).  
  
-   Questo comando non funziona in dischi dinamici o in Microsoft Reserved partizioni.  
  
## <a name="BKMK_examples"></a>Esempi  
Per impostare il campo tipo 0x07 e forzare il file system per smontare, digitare:  
  
```  
set id=0x07 override  
```  
  
Per impostare il campo di tipo sia una partizione di dati di base, digitare:  
  
```  
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7  
```  
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Chiave sintassi della riga di comando](command-line-syntax-key.md)  
  

  

