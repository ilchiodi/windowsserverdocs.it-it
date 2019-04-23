---
title: Verificare che sia disponibile spazio su disco sufficiente quando le macchine virtuali usano dischi rigidi virtuali differenze
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 71f99aab-f994-4022-9da0-d661965b95ac
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6d4d219402cdc321a75cc27d75ea7749eb6127e0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855572"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-differencing-virtual-hard-disks"></a>Verificare che sia disponibile spazio su disco sufficiente quando le macchine virtuali usano dischi rigidi virtuali differenze

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
*Uno o più macchine virtuali utilizzano dischi rigidi virtuali differenze.*  
  
## <a name="impact"></a>Impatto  
*In modo che può essere allocato quando si verificano operazioni di scrittura per i dischi rigidi virtuali, dischi rigidi virtuali differenze richiede spazio disponibile sul volume di hosting. Se lo spazio disponibile è esaurito, è possibile che una macchina virtuale che si basa sull'archiviazione fisica potrebbe risentirne. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Monitoraggio spazio disponibile su disco per garantire spazio sufficiente disponibile per l'espansione del disco rigido virtuale. Considerare l'unione dei dischi rigidi virtuali differenze nel relativo elemento padre. In Hyper-V Manager, controllare il disco differenze per determinare il disco rigido virtuale padre. Se si esegue l'unione di un disco differenze in un disco padre che è condiviso da altri dischi differenze, tale azione danneggerà la relazione tra gli altri dischi differenze e il disco padre, rendendo inutilizzabile. Dopo aver verificato che il disco rigido virtuale padre non è condiviso, è possibile utilizzare la modifica guidata disco per unire il disco differenze al disco rigido virtuale padre.*  
  


