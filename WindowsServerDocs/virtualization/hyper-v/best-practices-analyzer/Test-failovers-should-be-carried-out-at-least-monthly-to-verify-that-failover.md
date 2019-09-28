---
title: I failover di test devono essere eseguiti almeno mensilmente per verificare che il failover abbia esito positivo e che i carichi di lavoro della macchina virtuale funzioneranno come previsto dopo il failover
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 57a8aa50-e59e-4a4b-8571-1099d5a8eee4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c7f7c0e1076358ef417b4d98632bd65257989df3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393464"
---
# <a name="test-failovers-should-be-carried-out-at-least-monthly-to-verify-that-failover-will-succeed-and-that-virtual-machine-workloads-will-operate-as-expected-after-failover"></a>I failover di test devono essere eseguiti almeno mensilmente per verificare che il failover abbia esito positivo e che i carichi di lavoro della macchina virtuale funzioneranno come previsto dopo il failover

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016| 
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Operazioni|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Nessun failover di test in almeno un mese.*  
  
## <a name="impact"></a>Impatto  
*There non è in grado di confermare che un failover pianificato o non pianificato avrà esito positivo oppure le operazioni del carico di lavoro continueranno correttamente dopo un failover. Ciò influisca sulle macchine virtuali seguenti:*  
  
@no__t 0list di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Utilizzare la console di gestione di Hyper-V per eseguire un failover di test.*  
  


