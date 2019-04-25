---
title: Offrire i servizi di integrazione disponibile in tutte le macchine virtuali
description: Fornisce le istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2c4b2043-ad81-495e-aa7a-467f813bb3d2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c2b5137594f78980f87f6520ae4b4af8203aef32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883782"
---
# <a name="offer-all-available-integration-services-to-virtual-machines"></a>Offrire i servizi di integrazione disponibile in tutte le macchine virtuali

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Uno o più servizi di integrazione disponibili non sono abilitati nelle macchine virtuali.*  
  
## <a name="impact"></a>Impatto  
  
*Alcune funzionalità non saranno disponibili per le macchine virtuali seguenti:*  
  
\<elenco di nomi delle macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
  
*Se questa condizione è voluta, non è necessaria alcuna azione ulteriore. In caso contrario, prendere in considerazione tutti i servizi di integrazione nelle impostazioni di tali macchine virtuali.*  
  
La disponibilità di alcuni servizi di integrazione può essere gestita tramite impostazioni della macchina virtuale.   
  
#### <a name="to-manage-the-availability-of-integration-services-to-a-virtual-machine"></a>Per gestire la disponibilità dei servizi di integrazione in una macchina virtuale  
  
1.  Aprire la console di gestione di Hyper-V. Fare clic su **avviare**, scegliere **Strumenti di amministrazione**, quindi fare clic su **gestione di Hyper-V**.  
  
2.  Nel riquadro dei risultati, sotto **macchine virtuali**, selezionare la macchina virtuale che si desidera configurare.  
  
3.  Nel riquadro **Azioni** sotto il nome della macchina virtuale fare clic su **Impostazioni**.  
  
4.  In **Management**, fare clic su **Integration Services**.  
  
5.  Nell'elenco dei servizi di integrazione, selezionare la casella di controllo per ogni servizio che si desidera offrire alla macchina virtuale. Se una casella di controllo è disponibile, il servizio di integrazione specifico non è supportato dal sistema operativo guest che viene eseguito nella macchina virtuale.  
  

