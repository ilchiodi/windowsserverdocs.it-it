---
title: Windows 8 deve essere configurato con la quantità di memoria consigliata
description: Vengono fornite istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 0c739e7c-4403-4eff-9e69-213ba1ab7336
author: KBDAzure
ms.date: 10/03/2016
ms.openlocfilehash: a3f86ba70abc15d0d5f5db917e6daf53f631c56f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364499"
---
# <a name="windows-8-should-be-configured-with-the-recommended-amount-of-memory"></a>Windows 8 deve essere configurato con la quantità di memoria consigliata

>Si applica a: Windows Server 2016
  
Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema. 
 
## <a name="issue"></a>**Problema**  
*Una macchina virtuale che esegue Windows 8 è configurata con una quantità di RAM inferiore a quella consigliata, ovvero 1 GB.*  
  
## <a name="impact"></a>**Impatto**  
il sistema operativo guest *The e le applicazioni potrebbero non funzionare correttamente. Potrebbe non essere disponibile memoria sufficiente per eseguire più applicazioni contemporaneamente. Ciò influisca sulle macchine virtuali seguenti:*  
```  
<list of virtual machines>  
```  
## <a name="resolution"></a>**Soluzione**  
*Utilizzare la console di gestione di Hyper-V per aumentare la memoria allocata a questa macchina virtuale almeno 1 GB.*  
  
### <a name="increase-the-memory-using-hyper-v-manager"></a>Aumentare la memoria utilizzando la console di gestione di Hyper-V  
  
1.  Aprire la console di gestione di Hyper-V. Fare clic su **avviare**, scegliere **Strumenti di amministrazione**, quindi fare clic su **gestione di Hyper-V**.  
  
2.  Nel riquadro dei risultati, sotto **macchine virtuali**, selezionare la macchina virtuale che si desidera configurare. Lo stato della macchina virtuale deve essere indicato come **disattivato**. In caso contrario, fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi scegliere **Arresta**.  
  
3.  Nel riquadro **Azioni** sotto il nome della macchina virtuale fare clic su **Impostazioni**.  
  
4.  Nel riquadro di spostamento, fare clic su **memoria**.  
  
5.  Nella pagina **memoria** impostare la RAM di **avvio** su almeno 1 GB, quindi fare clic su **OK**.  
  
### <a name="increase-the-memory-using-windows-powershell"></a>Aumentare la memoria usando Windows PowerShell  
  
1.  Aprire Windows PowerShell. (Dal desktop fare clic su **Start** e iniziare a digitare **Windows PowerShell**).  
  
2.  Fare doppio clic su **Windows PowerShell** e fare clic su **Esegui come amministratore**.  
  
3.  Eseguire questo comando dopo aver sostituito \<MyVM > con il nome della macchina virtuale:  
  
```  
Set-VMMemory <MyVM> -StartupBytes 1GB  
```  
  
## <a name="see-also"></a>Vedere anche  
[Set-VMMemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


