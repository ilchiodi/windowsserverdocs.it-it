---
title: Le schede di visualizzazione devono essere abilitate nelle macchine virtuali per fornire funzionalità video
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: ac5992e6-3c0b-46c2-a48e-6ef37b679228
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0c515c7fb1ed160dfee1e1b7303022082e936157
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364915"
---
# <a name="display-adapters-should-be-enabled-in-virtual-machines-to-provide-video-capabilities"></a>Le schede di visualizzazione devono essere abilitate nelle macchine virtuali per fornire funzionalità video

>Si applica a: Windows Server 2016


  
*Per ulteriori informazioni sulle procedure consigliate e le analisi, vedere* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Il dispositivo video Microsoft Virtual Machine bus può essere disabilitato in una macchina virtuale.*  
  
Il dispositivo video Microsoft Virtual Machine bus è una scheda video virtuale ottimizzata per l'uso con macchine virtuali Hyper-V. Quando una macchina virtuale non è configurata per l'utilizzo del dispositivo video Microsoft Virtual Machine bus, viene utilizzata una scheda video legacy. Il dispositivo video Microsoft Virtual Machine bus offre prestazioni migliori rispetto a una scheda video legacy.  
  
## <a name="impact"></a>Impatto  
  
*Le prestazioni video per le macchine virtuali seguenti saranno ridotte:*  
  
@no__t 0list di nomi di macchina virtuale >  
  
## <a name="resolution"></a>Risoluzione  
  
*Usare Device Manager nel sistema operativo guest per abilitare il dispositivo video Microsoft Virtual Machine bus.*  
  
I passaggi necessari per usare Device Manager variano a seconda del sistema operativo. Per istruzioni, vedere la guida del sistema operativo guest.  
  


