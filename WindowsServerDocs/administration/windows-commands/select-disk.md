---
title: select disk
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a0da614b-09d9-433b-b4eb-9127f84431cb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 441e680f0a705255f6eba8c4a16db4a623e50b6b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834814"
---
# <a name="select-disk"></a>select disk

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleziona il disco specificato e sposta lo stato attivo a esso.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
select disk={ <n> | <disk path> | system | next }  
```  
  
> [!NOTE]  
> I parametri **<disk path>** , **System**e **Next** sono disponibili solo in Windows 7 e Windows Server 2008 R2.  
  
### <a name="parameters"></a>Parametri  
  
|  Parametro  |                                                                                                                                                                                                            Descrizione                                                                                                                                                                                                            |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     <n>     | Specifica il numero del disco per ricevere lo stato attivo. È possibile visualizzare i numeri per tutti i dischi nel computer utilizzando il **disco elenco** comando DiskPart. **Nota:** durante la configurazione di sistemi con più dischi, non utilizzare **disco selezionare\=0** per specificare il disco del sistema. Il computer può essere modificato i numeri di disco quando si riavvia, e diversi computer con la stessa configurazione di disco possono avere numeri di disco diverse. |
| <disk path> |                                                                                                                 Specifica il percorso del disco per ricevere lo stato attivo, ad esempio **PCIROOT\(0\)\#PCI\(0F02\)\#atA\(C00T00L00** \). Per visualizzare il percorso di un disco, selezionarlo e quindi digitare **disco dettaglio**.                                                                                                                  |
|   sistema    |                                 Nei computer BIOS, specifica che il disco 0 riceve lo stato attivo. Nei computer EFI, il disco contenente la partizione di sistema EFI \(ESP\) utilizzato per il processo di avvio corrente riceve lo stato attivo. Nei computer EFI, il comando avrà esito negativo se non esiste alcuna ESP, se sono presenti più ESP oppure il computer viene avviato da Ambiente preinstallazione di Windows \(\)Windows PE.                                  |
|    next     |                                                                                                                                     Dopo aver selezionato un disco, questo comando scorre tutti i dischi nell'elenco dei dischi. Quando si esegue questo comando, il disco successivo nell'elenco verrà visualizzato lo stato attivo.                                                                                                                                      |
  
## <a name="examples"></a><a name=BKMK_examples></a>Esempi  
Per spostare lo stato attivo sul disco 1, digitare:  
  
```  
select disk=1  
```  
  
Per selezionare un disco utilizzando il relativo percorso, digitare:  
  
```  
select disk=PCIROOT(0)#PCI(0100)#atA(C00T00L01)  
```  
  
Per spostare lo stato attivo per il disco del sistema, digitare:  
  
```  
select disk=system  
```  
  
Per spostare lo stato attivo per il disco successivo del computer, digitare:  
  
```  
select disk=next  
```  
  
## <a name="additional-references"></a>Altre informazioni di riferimento  
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

