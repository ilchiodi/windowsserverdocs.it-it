---
title: Più di una scheda di rete deve essere disponibile
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 59940e56-e06a-490f-90ea-cf30d9f80b09
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 678c0161e97b8dd022bbf0037d9add5de0281f77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884602"
---
# <a name="more-than-one-network-adapter-should-be-available"></a>Più di una scheda di rete deve essere disponibile

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Errore|  
|**Categoria**|Configurazione|  

Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.

## <a name="issue"></a>Problema  
  
*Questo server è configurato con una scheda di rete, che deve essere condivisi dal sistema operativo di gestione e tutte le macchine virtuali che richiedono l'accesso a una rete fisica.*  
  
## <a name="impact"></a>Impatto  
  
*Prestazioni di rete potrebbero essere compromesse nel sistema operativo di gestione.*  
  
## <a name="resolution"></a>Risoluzione  
  
*Aggiungere altre schede di rete al computer. Per riservare una scheda di rete per l'utilizzo esclusivo da sistema operativo di gestione, non configurata per l'uso con una rete virtuale esterna.*  
  
Per informazioni sull'aggiunta di una scheda di rete al computer, consultare la documentazione per il computer o la scheda di rete. Quindi, per riservare esclusivamente per il sistema operativo di gestione, non connetterlo a un commutatore virtuale.   
  


