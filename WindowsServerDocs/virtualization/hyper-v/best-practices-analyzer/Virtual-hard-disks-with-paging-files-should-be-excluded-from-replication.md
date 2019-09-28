---
title: Dischi rigidi virtuali con file di paging devono essere esclusi dalla replica
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 215e265d69af1b384d3461c627558ff0a59c8e91
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364548"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>Dischi rigidi virtuali con file di paging devono essere esclusi dalla replica

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Informazioni|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*I file di paging devono essere esclusi dalla replica, ma non sono stati esclusi dischi.*  
  
## <a name="impact"></a>Impatto  
i file @no__t 0Paging presentano un volume elevato di attività di input/output, che richiederà inutilmente risorse molto maggiori per partecipare alla replica. Ciò influisca sulle macchine virtuali seguenti: *  
  
@no__t 0list di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*If non è già stato fatto, creare un disco rigido virtuale distinto per il file di paging di Windows. Se è già stata completata la replica iniziale, utilizzare Hyper-V Manager per rimuovere la replica. Quindi, configurare di nuovo la replica ed escludere il disco rigido virtuale con il file di paging dalla replica.*  
  


