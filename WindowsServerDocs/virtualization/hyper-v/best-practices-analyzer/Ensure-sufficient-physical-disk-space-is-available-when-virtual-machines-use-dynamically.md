---
title: Verificare che sia disponibile spazio su disco sufficiente quando le macchine virtuali utilizzano dischi rigidi virtuali a espansione dinamica
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9e3e3e64-4b3a-4b9d-acf1-e4df61a04f1e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c38d7c11a05eef9d29097e625fec2830000cf550
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393638"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-dynamically-expanding-virtual-hard-disks"></a>Verificare che sia disponibile spazio su disco sufficiente quando le macchine virtuali utilizzano dischi rigidi virtuali a espansione dinamica

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
*Una o più macchine virtuali utilizzano dischi rigidi virtuali a espansione dinamica.*  
  
## <a name="impact"></a>Impatto  
i dischi rigidi virtuali a espansione @no__t 0Dynamically richiedono lo spazio disponibile nel volume host, in modo che lo spazio possa essere allocato quando si verificano scritture nei dischi rigidi virtuali. Se lo spazio disponibile è esaurito, è possibile che una macchina virtuale che si basa sull'archiviazione fisica potrebbe risentirne. Ciò influisca sulle macchine virtuali seguenti: *  
  
@no__t 0list di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Monitor spazio disponibile su disco per garantire che lo spazio disponibile sia sufficiente per l'espansione. È consigliabile arrestare la macchina virtuale e utilizzare la modifica guidata disco nella console di gestione di Hyper-V per convertire ogni disco rigido virtuale a espansione dinamica per la macchina virtuale in un disco rigido virtuale a dimensione fissa.*  
  


