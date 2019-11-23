---
title: Utilizzare tutte le funzioni virtuali per le reti quando sono disponibili
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8798a7021b3df0113b8d957340d6d688acead5c7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393354"
---
# <a name="use-all-virtual-functions-for-networking-when-they-are-available"></a>Utilizzare tutte le funzioni virtuali per le reti quando sono disponibili

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Alcune funzionalità di accelerazione hardware non vengono utilizzate*  
  
## <a name="impact"></a>Impatto  
*Questa configurazione potrebbe causare un utilizzo complessivo della CPU superiore al necessario. Le prestazioni di rete potrebbero non essere ottimali nelle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Prendere in considerazione la configurazione della scheda di rete virtuale per SR-IOV se l'hardware fisico supporta SR-IOV e se questa configurazione non è in conflitto con le funzionalità di rete richieste dalla macchina virtuale.*  
  


