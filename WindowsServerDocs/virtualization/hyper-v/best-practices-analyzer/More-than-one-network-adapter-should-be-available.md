---
title: Deve essere disponibile più di una scheda di rete
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 59940e56-e06a-490f-90ea-cf30d9f80b09
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6b043900c6fde4522e5805a1f0c1a635de335e31
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364805"
---
# <a name="more-than-one-network-adapter-should-be-available"></a>Deve essere disponibile più di una scheda di rete

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Error|  
|**Categoria**|Configurazione|  

Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.

## <a name="issue"></a>Problema  
  
*Questo server è configurato con una scheda di rete, che deve essere condivisa dal sistema operativo di gestione e da tutte le macchine virtuali che richiedono l'accesso a una rete fisica.*  
  
## <a name="impact"></a>Impatto  
  
*Le prestazioni di rete potrebbero risultare ridotte nel sistema operativo di gestione.*  
  
## <a name="resolution"></a>Risoluzione  
  
*Aggiungere altre schede di rete a questo computer. Per riservare una scheda di rete per l'utilizzo esclusivo da parte del sistema operativo di gestione, non configurarla per l'utilizzo con una rete virtuale esterna.*  
  
Per informazioni sull'aggiunta di una scheda di rete al computer, consultare la documentazione relativa al computer o alla scheda di rete. Quindi, per riservarlo esclusivamente per il sistema operativo di gestione, non connetterlo a un Commuter virtuale.   
  


