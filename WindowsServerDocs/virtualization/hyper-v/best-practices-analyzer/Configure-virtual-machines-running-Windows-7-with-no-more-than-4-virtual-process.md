---
title: Configurare le macchine virtuali che eseguono Windows 7 senza più di 4 processori virtuali
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8fcf0868-b543-4f94-aee7-35324346da55
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 14b5e0637ad2e6462e13f0e1f18af651bbcc5fc3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366275"
---
# <a name="configure-virtual-machines-running-windows-7-with-no-more-than-4-virtual-processors"></a>Configurare le macchine virtuali che eseguono Windows 7 senza più di 4 processori virtuali

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Errore|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>**Problema**  
*Una macchina virtuale che esegue Windows 7 è configurata con più di 4 processori virtuali.*  
  
## <a name="impact"></a>**Impatto**  
*Microsoft non supporta la configurazione delle macchine virtuali seguenti:*  
  
@no__t 0list di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Arrestare la macchina virtuale e rimuovere uno o più processori virtuali.*  
  
#### <a name="to-remove-virtual-processors"></a>Per rimuovere i processori virtuali  
  
1.  Aprire la console di gestione di Hyper-V. Fare clic su **avviare**, scegliere **Strumenti di amministrazione**, quindi fare clic su **gestione di Hyper-V**.  
  
2.  Nel riquadro dei risultati, sotto **macchine virtuali**, selezionare la macchina virtuale che si desidera configurare. Lo stato della macchina virtuale deve essere indicato come **disattivato**. In caso contrario, fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi scegliere **Arresta**.  
  
3.  Nel riquadro **Azioni** sotto il nome della macchina virtuale fare clic su **Impostazioni**.  
  
4.  Nel riquadro di spostamento, fare clic su **processore**.  
  
5.  Nella pagina **processore** impostare il numero di processori su **3** o meno, quindi fare clic su **OK**.  
  


