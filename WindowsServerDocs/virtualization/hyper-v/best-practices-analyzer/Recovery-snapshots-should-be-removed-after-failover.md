---
title: Gli snapshot di ripristino devono essere rimossi dopo il failover
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4663320df91019fc7dc1d8ca7ffdb2fcc3e0de42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837682"
---
# <a name="recovery-snapshots-should-be-removed-after-failover"></a>Gli snapshot di ripristino devono essere rimossi dopo il failover

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016| 
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Operazioni|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>**Problema**  
*Un failover macchina virtuale ha uno o più snapshot di ripristino.*  
  
## <a name="impact"></a>**Impact**  
*Potrebbe esaurirà lo spazio disponibile sul disco fisico che archivia i file di snapshot. In questo caso, è non possibile effettuare alcun operazioni disco nello spazio di archiviazione fisica. Qualsiasi macchina virtuale che si basa sull'archiviazione fisica potrebbe risentirne. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Per ognuno sulla macchina virtuale, usare il cmdlet Complete-VMFailover di Windows PowerShell per rimuovere gli snapshot di ripristino e indicare il completamento di failover.*  
  


