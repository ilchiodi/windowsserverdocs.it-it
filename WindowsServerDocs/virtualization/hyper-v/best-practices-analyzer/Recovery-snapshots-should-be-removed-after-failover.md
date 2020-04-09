---
title: Gli snapshot di ripristino devono essere rimossi dopo il failover
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: c995293ca67b4cad0837affa854fb4ac366856e1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861844"
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
*Lo spazio disponibile potrebbe esaurirsi sul disco fisico in cui sono archiviati i file di snapshot. In tal caso, non è possibile eseguire operazioni su disco aggiuntive sull'archiviazione fisica. Potrebbe essere interessata qualsiasi macchina virtuale che si basa sull'archiviazione fisica. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Per ogni macchina virtuale sottoposta a failover, usare il cmdlet Complete-VMFailover di Windows PowerShell per rimuovere gli snapshot di ripristino e indicare il completamento del failover.*  
  


