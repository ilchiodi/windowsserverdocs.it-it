---
title: Dischi rigidi virtuali con file di paging devono essere esclusi dalla replica
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8b6289a82c83f3dcfc0de299250ce19ee3782678
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850122"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>Dischi rigidi virtuali con file di paging devono essere esclusi dalla replica

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Informazioni|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*I file di paging devono essere esclusi dalla replica, ma nessun disco è state escluse.*  
  
## <a name="impact"></a>Impatto  
*File di paging riscontrare un volume elevato di attività di input/output, che richiedono inutilmente risorse molto maggiori per partecipare alla replica. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Se non è già stato fatto, creare un disco rigido virtuale distinto per il file di paging di Windows. Se è già stata completata la replica iniziale, utilizzare Hyper-V Manager per rimuovere la replica. Quindi, configurare la replica ed escludere il disco rigido virtuale con il file di paging dalla replica.*  
  


