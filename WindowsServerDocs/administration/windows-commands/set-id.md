---
title: Imposta ID
description: Argomento di riferimento per l'ID set DiskPart, che modifica il campo tipo di partizione per la partizione con lo stato attivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5793d7ad-827e-4285-b2c6-ae60eeb0e886
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ae0b23cf17974c73575948177a885ee14d70c5a0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721921"
---
# <a name="set-id"></a>Imposta ID

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il comando Diskpart imposta ID modifica il campo tipo di partizione per la partizione con lo stato attivo.  
  
> [!IMPORTANT]  
> Questo comando deve essere utilizzato per gli original equipment manufacturer \(OEM\) solo. Modifica di campi di tipo di partizione con questo parametro potrebbe non riuscire o non essere in grado di eseguire l'avvio del computer. A meno che non si sia un OEM o un esperto di dischi GPT, è consigliabile non modificare i campi del tipo di partizione nei dischi GPT utilizzando questo parametro. Usare invece sempre il comando [Create Partition EFI](create-partition-efi.md) per creare partizioni di sistema EFI, il comando [Create Partition MSR](create-partition-msr.md) per creare partizioni riservate Microsoft e il comando [create partition primary](create-partition-primary.md) senza il parametro ID per creare partizioni primarie nei dischi GPT.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
set id={ <byte> | <GUID> } [override] [noerr]  
```  
  
### <a name="parameters"></a>Parametri  
  
| Parametro |                                                                                                                                                                                                                                                                                                                                                                   Descrizione                                                                                                                                                                                                                                                                                                                                                                   |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <byte>   |                                                                                                                                                                                                       per record \(di avvio principale\) MBR dischi, specifica il nuovo valore per il campo tipo, in formato esadecimale, per la partizione. Con questo parametro, ad eccezione di tipo 0x42, che specifica una partizione di gestione dischi LOGICI, è possibile specificare qualsiasi tipo di partizione. Si noti che il 0x principale viene omesso quando si specifica il tipo di partizione esadecimale.                                                                                                                                                                                                       |
|  <GUID>   | per i dischi GPT \(\) della tabella di partizione GUID, specifica il nuovo valore GUID per il campo tipo per la partizione. GUID riconosciuto includono:<p>-Partizione di sistema EFI: c12a7328\-f81f\-11d 2\-ba4b\-00a0c93ec93b<br />-Partizione di dati di base: ebd0a0a2\-b9e5\-4433\-c 87 0\-68b6b72699c7<p>Qualsiasi tipo di partizione GUID può essere specificato con questo parametro ad eccezione dei seguenti:<p>-Partizione Microsoft Reserved: e3c9e316\-0b5c\-4db8\-d 817\-f92df00215ae<br />-Partizione metadati LDM su un disco dinamico: 5808c8aa\-7e8f\-42e0\-d 85 2\-e1e90434cfb3<br />-Partizione di dati Gestione dischi LOGICI su un disco dinamico: af9b60a0\-1431\-4f62\-bc68\-3311714a69ad<br />-Cluster partizione metadati: db97dba9\-0840\-4bae\-97f0\-ffb9a327c7e1 |
| override  |                                                                forza la disinstallazione del file system sul volume prima di modificare il tipo di partizione. Quando si esegue il **id Set** comando DiskPart tenta di bloccare e smontare il file system sul volume. Se **override** non è specificato, e la chiamata di bloccare il file system non riesce \(ad esempio, poiché non esiste un handle aperto\), l'operazione avrà esito negativo. Quando **sostituzione** viene specificato, DiskPart forza la disinstallazione anche se ha esito negativo della chiamata a bloccare il file system e tutti gli handle aperti per il volume non saranno più validi.<p>Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.                                                                 |
|   NOERR   |                                                                                                                                                                                                                                                                    Utilizzato solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.                                                                                                                                                                                                                                                                    |
  
## <a name="remarks"></a>Osservazioni  
  
-   Oltre alle limitazioni menzionate in precedenza, ma non controlla la validità del valore specificato \(tranne assicurarsi che sia un byte in formato esadecimale o un GUID\).  
  
-   Questo comando non funziona in dischi dinamici o in Microsoft Reserved partizioni.  
  
## <a name="examples"></a>Esempi  
Per impostare il campo tipo 0x07 e forzare il file system per smontare, digitare:  
  
```  
set id=0x07 override  
```  
  
Per impostare il campo di tipo sia una partizione di dati di base, digitare:  
  
```  
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7  
```  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

