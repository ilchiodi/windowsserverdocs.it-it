---
title: Windows 10 deve essere configurato con almeno la quantità minima di memoria
description: Fornisce le istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e4f5bd2f-b5be-4d43-80e0-0cf198182791
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e621750049666a916d99cf843d3a7a16069773be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824122"
---
# <a name="windows-10-should-be-configured-with-at-least-the-minimum-amount-of-memory"></a>Windows 10 deve essere configurato con almeno la quantità minima di memoria

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Errore|  
|**Categoria**|Configurazione|  
  
Le sezioni seguenti forniscono informazioni dettagliate sul problema. Il corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per lo specifico problema.  
  
## <a name="issue"></a>**Problema**  
*Una macchina virtuale che esegue Windows 10 è configurata con minore rispetto alla quantità minima di RAM, è 512 MB.*  
  
## <a name="impact"></a>**Impact**  
*Il sistema operativo guest nelle macchine virtuali seguenti potrebbero non essere eseguiti o potrebbero essere eseguiti unreliably:*  
```  
<list of virtual machines>  
```  
## <a name="resolution"></a>**Soluzione**  
*Utilizzare Hyper-V Manager per aumentare la memoria allocata alla macchina virtuale di almeno 512 MB.*  
  
#### <a name="increase-the-memory-using-hyper-v-manager"></a>Aumentare la memoria tramite Gestione di Hyper-V  
  
1.  Aprire la console di gestione di Hyper-V. Fare clic su **avviare**, scegliere **Strumenti di amministrazione**, quindi fare clic su **gestione di Hyper-V**.  
  
2.  Nel riquadro dei risultati, sotto **macchine virtuali**, selezionare la macchina virtuale che si desidera configurare. Lo stato della macchina virtuale dovrebbe essere elencato come **disattivata**. In caso contrario, fare clic sulla macchina virtuale e quindi fare clic su **Spegni**.  
  
3.  Nel riquadro **Azioni** sotto il nome della macchina virtuale fare clic su **Impostazioni**.  
  
4.  Nel riquadro di spostamento, fare clic su **memoria**.  
  
5.  Nel **memoria** pagina, impostare il **RAM di avvio** almeno 512 MB e quindi fare clic su **OK**.  
  
### <a name="increase-the-memory-using-windows-powershell"></a>Aumentare la memoria con Windows PowerShell  
  
1.  Aprire Windows PowerShell. (Dal desktop, fare clic su **avviare** e iniziare a digitare **Windows PowerShell**.)  
  
2.  Fare doppio clic su **Windows PowerShell** e fare clic su **Esegui come amministratore**.  
  
3.  Eseguire questo comando dopo aver sostituito \<MyVM > con il nome della macchina virtuale:  
  
```  
Set-VMMemory <MyVM> -StartupBytes 512MB  
```  
  
## <a name="see-also"></a>Vedere anche  
[Set-VMMemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


