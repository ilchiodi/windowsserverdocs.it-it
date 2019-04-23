---
title: Utilizzare tutte le funzioni virtuali per le reti quando sono disponibili
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3ad120ffa689f1f7dcae832432e216ebda57e62f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877792"
---
# <a name="use-all-virtual-functions-for-networking-when-they-are-available"></a>Utilizzare tutte le funzioni virtuali per le reti quando sono disponibili

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Alcune funzionalità di accelerazione hardware non presentano un utilizzo*  
  
## <a name="impact"></a>Impatto  
*Questa configurazione potrebbe causare l'utilizzo globale della CPU superiori al necessario. Prestazioni di rete potrebbero non essere ottimale nelle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Si consiglia di configurare la scheda di rete virtuale per SR-IOV se l'hardware fisico supporta SR-IOV e se questa configurazione non è in conflitto con le funzionalità di rete necessari per la macchina virtuale.*  
  


