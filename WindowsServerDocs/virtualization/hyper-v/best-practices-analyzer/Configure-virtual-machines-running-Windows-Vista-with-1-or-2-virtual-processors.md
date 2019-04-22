---
title: Configurare le macchine virtuali in esecuzione Windows Vista con 1 o 2 processori virtuali
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e562bce3-fd68-42c9-821c-12022ae4746c
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: fae7d5e437fd83b9c00afcceaaf0eb7e8a7b909b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812072"
---
# <a name="configure-virtual-machines-running-windows-vista-with-1-or-2-virtual-processors"></a>Configurare le macchine virtuali in esecuzione Windows Vista con 1 o 2 processori virtuali

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Configurazione|  
|**Categoria**|Errore|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Una macchina virtuale che esegue Windows Vista è configurata con più di 2 processori virtuali.*  
  
## <a name="impact"></a>Impatto  
  
*Microsoft non supporta la configurazione delle macchine virtuali seguenti:*  
  
\<elenco di nomi delle macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
  
*Arrestare la macchina virtuale e rimuovere uno o più processori virtuali.*  
  
### <a name="to-remove-virtual-processors"></a>Per rimuovere i processori virtuali  
  
1.  Aprire la console di gestione di Hyper-V. Fare clic su **avviare**, scegliere **Strumenti di amministrazione**, quindi fare clic su **gestione di Hyper-V**.  
  
2.  Nel riquadro dei risultati, sotto **macchine virtuali**, selezionare la macchina virtuale che si desidera configurare. Lo stato della macchina virtuale dovrebbe essere elencato come **disattivata**. In caso contrario, fare clic sulla macchina virtuale e quindi fare clic su **Spegni**.  
  
3.  Nel riquadro **Azioni** sotto il nome della macchina virtuale fare clic su **Impostazioni**.  
  
4.  Nel riquadro di spostamento, fare clic su **processore**.  
  
5.  Nel **processore** pagina, impostare il numero di processori da **1** o **2** e quindi fare clic su **OK**.  
  


