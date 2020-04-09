---
title: Utilizzare tutte le funzioni virtuali per le reti quando sono disponibili
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 0fab06ae21a4632df73b7a4d8b17b12665ffed98
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854214"
---
# <a name="use-all-virtual-functions-for-networking-when-they-are-available"></a>Utilizzare tutte le funzioni virtuali per le reti quando sono disponibili

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
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
  


