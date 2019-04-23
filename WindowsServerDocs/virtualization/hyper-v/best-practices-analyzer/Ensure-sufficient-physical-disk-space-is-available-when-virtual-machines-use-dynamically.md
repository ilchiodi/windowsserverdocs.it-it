---
title: Verificare che sia disponibile spazio su disco sufficiente quando le macchine virtuali utilizzano dischi rigidi virtuali a espansione dinamica
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9e3e3e64-4b3a-4b9d-acf1-e4df61a04f1e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 09e481b99594ac543dadab2b60bf9b3f4c29e54b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883292"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-dynamically-expanding-virtual-hard-disks"></a>Verificare che sia disponibile spazio su disco sufficiente quando le macchine virtuali utilizzano dischi rigidi virtuali a espansione dinamica

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
*Uno o più macchine virtuali utilizzano dischi rigidi virtuali a espansione dinamica.*  
  
## <a name="impact"></a>Impatto  
*In modo che può essere allocato quando si verificano operazioni di scrittura per i dischi rigidi virtuali, dischi rigidi virtuali a espansione dinamica richiede spazio disponibile sul volume di hosting. Se lo spazio disponibile è esaurito, è possibile che una macchina virtuale che si basa sull'archiviazione fisica potrebbe risentirne. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Monitoraggio spazio disponibile su disco per garantire spazio sufficiente disponibile per l'espansione. È consigliabile arrestare la macchina virtuale e usare la modifica guidata disco in Hyper-V Manager per convertire ogni disco rigido virtuale a espansione dinamica per questa macchina virtuale su un disco rigido virtuale con dimensione fissato.*  
  


