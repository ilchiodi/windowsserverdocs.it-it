---
title: Verificare che sia disponibile spazio su disco sufficiente quando le macchine virtuali usano dischi rigidi virtuali differenze
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 71f99aab-f994-4022-9da0-d661965b95ac
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3827d149d2d691b4ecd7fe6ae8f6d7255c85a31c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364867"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-differencing-virtual-hard-disks"></a>Verificare che sia disponibile spazio su disco sufficiente quando le macchine virtuali usano dischi rigidi virtuali differenze

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Una o più macchine virtuali utilizzano dischi rigidi virtuali differenze.*  
  
## <a name="impact"></a>Impatto  
i dischi rigidi virtuali *Differencing richiedono lo spazio disponibile nel volume di hosting, in modo che lo spazio possa essere allocato quando si verificano scritture nei dischi rigidi virtuali. Se lo spazio disponibile è esaurito, è possibile che una macchina virtuale che si basa sull'archiviazione fisica potrebbe risentirne. Ciò influisca sulle macchine virtuali seguenti:*  
  
@no__t 0list di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Monitor spazio disponibile su disco per garantire che lo spazio disponibile sia sufficiente per l'espansione del disco rigido virtuale. Considerare l'unione dei dischi rigidi virtuali differenze nel relativo elemento padre. In Hyper-V Manager, controllare il disco differenze per determinare il disco rigido virtuale padre. Se si esegue l'unione di un disco differenze in un disco padre che è condiviso da altri dischi differenze, tale azione danneggerà la relazione tra gli altri dischi differenze e il disco padre, rendendo inutilizzabile. Dopo aver verificato che il disco rigido virtuale padre non è condiviso, è possibile utilizzare la modifica guidata disco per unire il disco differenze al disco rigido virtuale padre.*  
  


