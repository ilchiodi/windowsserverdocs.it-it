---
title: Configurare le macchine virtuali in esecuzione Windows Vista con 1 o 2 processori virtuali
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e562bce3-fd68-42c9-821c-12022ae4746c
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f0fd83122ce148cfa97147a352ebef4f7cc443cf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364926"
---
# <a name="configure-virtual-machines-running-windows-vista-with-1-or-2-virtual-processors"></a>Configurare le macchine virtuali in esecuzione Windows Vista con 1 o 2 processori virtuali

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Configurazione|  
|**Categoria**|Errore|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Una macchina virtuale che esegue Windows Vista è configurata con più di due processori virtuali.*  
  
## <a name="impact"></a>Impatto  
  
*Microsoft non supporta la configurazione delle macchine virtuali seguenti:*  
  
@no__t 0list di nomi di macchina virtuale >  
  
## <a name="resolution"></a>Risoluzione  
  
*Arrestare la macchina virtuale e rimuovere uno o più processori virtuali.*  
  
### <a name="to-remove-virtual-processors"></a>Per rimuovere i processori virtuali  
  
1.  Aprire la console di gestione di Hyper-V. Fare clic su **avviare**, scegliere **Strumenti di amministrazione**, quindi fare clic su **gestione di Hyper-V**.  
  
2.  Nel riquadro dei risultati, sotto **macchine virtuali**, selezionare la macchina virtuale che si desidera configurare. Lo stato della macchina virtuale deve essere indicato come **disattivato**. In caso contrario, fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi scegliere **Arresta**.  
  
3.  Nel riquadro **Azioni** sotto il nome della macchina virtuale fare clic su **Impostazioni**.  
  
4.  Nel riquadro di spostamento, fare clic su **processore**.  
  
5.  Nel **processore** pagina, impostare il numero di processori da **1** o **2** e quindi fare clic su **OK**.  
  


