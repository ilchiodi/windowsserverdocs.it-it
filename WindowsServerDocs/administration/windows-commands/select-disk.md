---
title: select disk
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a0da614b-09d9-433b-b4eb-9127f84431cb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2da74afda7c15145327b4d64f5c0e97e4f9b10cc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441440"
---
# <a name="select-disk"></a>select disk

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleziona il disco specificato e sposta lo stato attivo a esso.  
  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
select disk={ <n> | <disk path> | system | next }  
```  
  
> [!NOTE]  
> Il **<disk path>** , **system**, e **Avanti** parametri sono disponibili solo in Windows 7 e Windows Server 2008 R2.  
  
## <a name="parameters"></a>Parametri  
  
|  Parametro  |                                                                                                                                                                                                            Descrizione                                                                                                                                                                                                            |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     <n>     | Specifica il numero del disco per ricevere lo stato attivo. È possibile visualizzare i numeri per tutti i dischi nel computer utilizzando il **disco elenco** comando DiskPart. **Nota:** Quando si configurano i sistemi con più dischi, non utilizzare **disco selezionare\=0** per specificare il disco del sistema. Il computer può essere modificato i numeri di disco quando si riavvia, e diversi computer con la stessa configurazione di disco possono avere numeri di disco diverse. |
| <disk path> |                                                                                                                 Specifica il percorso del disco per ricevere lo stato attivo, ad esempio, **PCIROOT\(0\)\#PCI\(0F02\)\#atA\(C00T00L00\)** . Per visualizzare il percorso di un disco, selezionarlo e quindi digitare **disco dettaglio**.                                                                                                                  |
|   sistema    |                                 Nei computer BIOS, specifica che il disco 0 riceve lo stato attivo. Nei computer EFI, il disco contenente la partizione di sistema EFI \(ESP\) utilizzato per il processo di avvio corrente riceve lo stato attivo. Nei computer EFI, il comando avrà esito negativo se non esiste alcun ESP, se è presente più di un ESP oppure il computer viene avviato da Ambiente preinstallazione di Windows \(Windows PE\).                                  |
|    Avanti     |                                                                                                                                     Dopo aver selezionato un disco, questo comando scorre tutti i dischi nell'elenco dei dischi. Quando si esegue questo comando, il disco successivo nell'elenco verrà visualizzato lo stato attivo.                                                                                                                                      |
  
## <a name="BKMK_examples"></a>Esempi  
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
  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
  

  

