---
title: La risincronizzazione della replica deve essere pianificata per le ore
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2d6c18b7e37c5d17f56f41c7ff03ed8796457de0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840022"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>La risincronizzazione della replica deve essere pianificata per le ore

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Operazioni|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*La risincronizzazione della replica per le macchine virtuali primarie non è pianificata per le ore non di punta.*  
  
## <a name="impact"></a>Impatto  
*È più lungo una macchina virtuale è in uno stato che richiedono la risincronizzazione, più è lunga i file di log di replica aumentano e verificano le modifiche non replicate più nelle macchine virtuali primarie. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Per modificare le impostazioni di replica per la macchina virtuale per eseguire la risincronizzazione automaticamente durante gli orari, utilizzare Gestione Hyper-V.*  
  


