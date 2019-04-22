---
title: Configurare un criterio per limitare il traffico di replica sulla rete
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 82cb1aef-cdc3-4d0a-88d4-ef497ab79606
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2c1f1865fa1d611c0b5baaf981140f9807b51458
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818692"
---
# <a name="configure-a-policy-to-throttle-the-replication-traffic-on-the-network"></a>Configurare un criterio per limitare il traffico di replica sulla rete

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Potrebbe non esserci un limite sulla quantità di larghezza di banda che la replica può utilizzare.*  
  
## <a name="impact"></a>Impatto  
*Larghezza di banda di rete potrebbe diventare completamente dominato da traffico di replica, influire sulle altre attività di rete critiche. Questo influisce sulle porte seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Se si usa un altro metodo per limitare il traffico di rete, è possibile ignorarlo. In caso contrario, usare Editor criteri di gruppo per configurare un criterio che limiterà il traffico di rete alla porta appropriata del server di Replica.*  
  
  


