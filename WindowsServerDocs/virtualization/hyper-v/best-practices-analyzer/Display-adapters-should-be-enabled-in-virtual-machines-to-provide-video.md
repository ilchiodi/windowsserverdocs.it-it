---
title: Schede video devono essere abilitate nelle macchine virtuali per fornire funzionalità video
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: ac5992e6-3c0b-46c2-a48e-6ef37b679228
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8d61461db471a876ddf46c1e5fec6992ffa80373
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870692"
---
# <a name="display-adapters-should-be-enabled-in-virtual-machines-to-provide-video-capabilities"></a>Schede video devono essere abilitate nelle macchine virtuali per fornire funzionalità video

>Si applica a: Windows Server 2016


  
*Per altre informazioni sulle procedure consigliate e le analisi, vedere* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Il dispositivo di Video di Microsoft del Bus di macchina virtuale potrebbe essere disabilitato in una macchina virtuale.*  
  
Dispositivo Video del Bus di macchina virtuale di Microsoft è una scheda video virtuale ottimizzata per l'uso con macchine virtuali Hyper-V. Quando una macchina virtuale non è configurata per usare il dispositivo di Video del Bus di macchina virtuale di Microsoft, viene utilizzata una scheda video legacy. Dispositivo Video del Bus di macchina virtuale di Microsoft offre prestazioni migliori rispetto a una scheda video legacy.  
  
## <a name="impact"></a>Impatto  
  
*Saranno danneggiata video sulle prestazioni per le macchine virtuali seguenti:*  
  
\<elenco di nomi delle macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
  
*Utilizzare Gestione dispositivi nel sistema operativo guest per abilitare il dispositivo di Video del Bus di macchina virtuale di Microsoft.*  
  
I passaggi necessari per usare Gestione dispositivi variano a seconda del sistema operativo. Per istruzioni, vedere la Guida del sistema operativo guest.  
  


