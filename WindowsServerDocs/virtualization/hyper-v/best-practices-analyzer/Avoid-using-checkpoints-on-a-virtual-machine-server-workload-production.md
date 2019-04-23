---
title: Evitare di usare i checkpoint in una macchina virtuale che esegue un carico di lavoro server in un ambiente di produzione
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1be75890-d316-495a-b9b7-be75fc1aac10
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 166ef839a40452cc4156144e10e9c666e7ce3472
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856152"
---
# <a name="avoid-using-checkpoints-on-a-virtual-machine-that-runs-a-server-workload-in-a-production-environment"></a>Evitare di usare i checkpoint in una macchina virtuale che esegue un carico di lavoro server in un ambiente di produzione

>Si applica a: Windows Server 2016


  
*Per altre informazioni sulle procedure consigliate e le analisi, vedere* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Operazioni|  

Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.

> [!NOTE]  
> In Windows Server 2012 R2, snapshot delle macchine virtuali sono stati rinominati come checkpoint macchina virtuale nella console di gestione in modo che corrisponda la terminologia usata in System Center Virtual Machine Management Hyper-V. Per informazioni dettagliate, vedere [Checkpoints and Snapshots Overview](https://technet.microsoft.com/library/dn818483.aspx).  
  
## <a name="issue"></a>Problema  
  
*Una macchina virtuale con uno o più punti di arresto è stata individuata.*  
  
## <a name="impact"></a>Impatto  
  
*Potrebbe esaurirà lo spazio disponibile sul disco fisico che archivia i file di checkpoint. In questo caso, è non possibile effettuare alcun operazioni disco nello spazio di archiviazione fisica. Qualsiasi macchina virtuale che si basa sull'archiviazione fisica potrebbe risentirne.*  
  
Se si esaurisce lo spazio su disco fisico, qualsiasi macchina virtuale in esecuzione con i checkpoint o dischi rigidi virtuali archiviati in tale disco può essere sospeso automaticamente. Gestione di Hyper-V Mostra lo stato di tali macchine virtuali come "paused-critical".  
  
## <a name="resolution"></a>Risoluzione  
  
*Se la macchina virtuale è in esecuzione un carico di lavoro server in un ambiente di produzione, portare offline la macchina virtuale e quindi usare Hyper-V Manager per applicare o eliminare i checkpoint. Per eliminare i checkpoint, è necessario arrestare la macchina virtuale per completare il processo.*  
  
> [!NOTE]  
> I checkpoint di produzione sono ora disponibili come alternativa a checkpoint standard. Per informazioni dettagliate, vedere [scegliere tra i checkpoint standard o di produzione](../manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md).  
  


