---
title: La risincronizzazione della replica deve essere pianificata per le ore
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 379f8c8cd6744fe5db176efb55a84f231ce45857
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393506"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>La risincronizzazione della replica deve essere pianificata per le ore

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Operazioni|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*La risincronizzazione della replica per le macchine virtuali primarie non è pianificata per le ore non di punta.*  
  
## <a name="impact"></a>Impatto  
*Più a lungo una macchina virtuale si trova in uno stato che richiede la risincronizzazione, più è grande la crescita dei file di log di replica e le modifiche non replicate vengono eseguite nelle macchine virtuali primarie. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Utilizzare la console di gestione di Hyper-V per modificare le impostazioni di replica per la macchina virtuale in modo da eseguire automaticamente la risincronizzazione durante gli orari di minore utilizzo.*  
  


