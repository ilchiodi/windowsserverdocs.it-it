---
title: Gli snapshot di ripristino devono essere rimossi dopo il failover
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4b8574956fb1b46ca0cf9678187fffcd68c2d261
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393533"
---
# <a name="recovery-snapshots-should-be-removed-after-failover"></a>Gli snapshot di ripristino devono essere rimossi dopo il failover

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016| 
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Operazioni|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>**Problema**  
*Una macchina virtuale di cui è stato eseguito il failover ha uno o più snapshot di ripristino.*  
  
## <a name="impact"></a>**Impatto**  
@no__t 0Available spazio potrebbe esaurirsi sul disco fisico in cui sono archiviati i file di snapshot. In questo caso, è non possibile effettuare alcun operazioni disco nello spazio di archiviazione fisica. Qualsiasi macchina virtuale che si basa sull'archiviazione fisica potrebbe risentirne. Ciò influisca sulle macchine virtuali seguenti: *  
  
@no__t 0list di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Per ogni macchina virtuale sottoposta a failover, usare il cmdlet Complete-VMFailover di Windows PowerShell per rimuovere gli snapshot di ripristino e indicare il completamento del failover.*  
  


