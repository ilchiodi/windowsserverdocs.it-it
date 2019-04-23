---
title: Porte seriali non devono essere configurate in macchine virtuali di generazione 2
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 87061193-dd3f-4398-aa5d-4cee83cadfa3
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 58c3fc5f975b85ce17ac5f7cca4930ec9e851e07
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877382"
---
# <a name="serial-ports-should-not-be-configured-on-generation-2-virtual-machines"></a>Porte seriali non devono essere configurate in macchine virtuali di generazione 2

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>**Problema**  
*Generazione di uno o più 2 macchine virtuali hanno una porta seriale configurata.*  
  
## <a name="impact"></a>**Impact**  
*Potrebbero influire sulle prestazioni per le macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Se questa condizione è voluta, non è necessaria alcuna azione ulteriore. In caso contrario, è consigliabile usare Gestione di Hyper-V o Windows PowerShell per rimuovere la stringa di connessione da porte seriali nella macchina virtuale.*  
  


