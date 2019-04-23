---
title: Failover di test dovrà essere eseguito al termine della replica iniziale
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: cea7eeaa-c1a7-4f87-89be-d4e1208c546f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 347b089330548e5fb2631e43ca66c96bb6bc4e9d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880412"
---
# <a name="test-failover-should-be-attempted-after-initial-replication-is-complete"></a>Failover di test dovrà essere eseguito al termine della replica iniziale

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Operazioni|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="problem"></a>Problema  
*Non si è verificato alcun failover di test in almeno un mese.*  
  
## <a name="impact"></a>Impatto  
*Non vi è alcuna conferma che un failover pianificato o avranno esito positivo o operazioni di carico di lavoro continueranno correttamente dopo un failover. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Utilizzare Gestione Hyper-V per eseguire un failover di test.*  
  


