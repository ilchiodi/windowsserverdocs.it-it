---
title: Una macchina virtuale in esecuzione Windows Vista e configurato con la memoria dinamica deve utilizzare valori consigliati per le impostazioni della memoria
description: Vengono fornite istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c35f08b2-e624-4811-a159-c1e5bb6d5281
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0a9ce14c31d2ea6e26b03d5c430a428474255b84
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366584"
---
# <a name="a-virtual-machine-running-windows-vista-and-configured-with-dynamic-memory-should-use-recommended-values-for-memory-settings"></a>Una macchina virtuale in esecuzione Windows Vista e configurato con la memoria dinamica deve utilizzare valori consigliati per le impostazioni della memoria

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Una o più macchine virtuali sono configurate per l'utilizzo di memoria dinamica con minore rispetto alla quantità di memoria consigliata per Windows Vista.*  
  
### <a name="impact"></a>Impatto  
*Il sistema operativo guest nelle macchine virtuali seguenti potrebbe non essere eseguito o potrebbe non essere eseguito in modo affidabile:*  
  
@no__t 0list di macchine virtuali >  
      
## <a name="resolution"></a>Risoluzione  
*Usare la console di gestione di Hyper-V o Windows PowerShell per aumentare la memoria minima per almeno 256 MB, memoria di avvio per almeno 512 MB e memoria massima per almeno 1 GB.*  
  
#### <a name="increase-memory-using-hyper-v-manager"></a>Aumentare la memoria tramite Gestione di Hyper-V  
  
1.  Aprire la console di gestione di Hyper-V. (Da Server Manager, fare clic su **Strumenti** > **Hyper-V Manager**.)  
  
2.  Dall'elenco di macchine virtuali, fare doppio clic su quello desiderato, quindi fare clic su **impostazioni**.  
  
3.  Nel riquadro di spostamento, fare clic su **memoria**.  
  
4.  Modifica il **RAM** su almeno 512 MB.  
  
5.  In **la memoria dinamica**,  modificare il **RAM minima** su almeno 256 MB e **RAM massima** a 1 GB.  
  
6.  Fare clic su **OK**.  
  
### <a name="increase-memory-using-windows-powershell"></a>Aumentare la memoria con Windows PowerShell  
  
1.  Aprire Windows PowerShell. (Dal desktop fare clic su **Start** e iniziare a digitare **Windows PowerShell**).  
  
2.  Fare doppio clic su **Windows PowerShell** e fare clic su **Esegui come amministratore**.  
  
3.  Eseguire un comando simile al seguente, sostituendo MyVM con il nome della macchina virtuale e la memoria di valori con almeno i valori riportati di seguito.  
  
```  
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 1GB -MinimumBytes 256MB -StartupBytes 512MB  
```  
  


