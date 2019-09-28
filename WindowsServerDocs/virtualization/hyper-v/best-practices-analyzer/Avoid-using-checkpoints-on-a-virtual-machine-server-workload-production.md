---
title: Evitare l'utilizzo di checkpoint in una macchina virtuale che esegue un carico di lavoro del server in un ambiente di produzione
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1be75890-d316-495a-b9b7-be75fc1aac10
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f2486093e31143b7493665d3d1254f7034ad1415
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365222"
---
# <a name="avoid-using-checkpoints-on-a-virtual-machine-that-runs-a-server-workload-in-a-production-environment"></a>Evitare l'utilizzo di checkpoint in una macchina virtuale che esegue un carico di lavoro del server in un ambiente di produzione

>Si applica a: Windows Server 2016


  
*Per ulteriori informazioni sulle procedure consigliate e le analisi, vedere* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Operazioni|  

Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.

> [!NOTE]  
> In Windows Server 2012 R2 gli snapshot delle macchine virtuali sono stati rinominati come Checkpoint della macchina virtuale nella console di gestione di Hyper-V in modo che corrispondano alla terminologia usata in System Center Virtual Machine Management. Per informazioni dettagliate, vedere [Panoramica di checkpoint e snapshot](https://technet.microsoft.com/library/dn818483.aspx).  
  
## <a name="issue"></a>Problema  
  
*È stata rilevata una macchina virtuale con uno o più checkpoint.*  
  
## <a name="impact"></a>Impatto  
  
@no__t 0Available spazio potrebbe esaurirsi sul disco fisico in cui sono archiviati i file di checkpoint. In questo caso, è non possibile effettuare alcun operazioni disco nello spazio di archiviazione fisica. Potrebbe essere interessata qualsiasi macchina virtuale che si basa sull'archiviazione fisica. *  
  
Se lo spazio fisico su disco è esaurito, tutte le macchine virtuali in esecuzione con checkpoint o dischi rigidi virtuali archiviati nel disco potrebbero essere sospese automaticamente. La console di gestione di Hyper-V Mostra lo stato di queste macchine virtuali come "sospeso-critico".  
  
## <a name="resolution"></a>Risoluzione  
  
*If la macchina virtuale esegue un carico di lavoro del server in un ambiente di produzione, disconnettere la macchina virtuale e quindi usare la console di gestione di Hyper-V per applicare o eliminare i checkpoint. Per eliminare i checkpoint, è necessario arrestare la macchina virtuale per completare il processo.*  
  
> [!NOTE]  
> I checkpoint di produzione sono ora disponibili come alternativa ai checkpoint standard. Per informazioni dettagliate, vedere [scegliere tra i checkpoint standard o di produzione](../manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md).  
  


