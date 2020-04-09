---
title: Porte seriali non devono essere configurate in macchine virtuali di generazione 2
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 87061193-dd3f-4398-aa5d-4cee83cadfa3
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: d9cfb8db2dc3fdff32544670d9443632c2d2264e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861784"
---
# <a name="serial-ports-should-not-be-configured-on-generation-2-virtual-machines"></a>Porte seriali non devono essere configurate in macchine virtuali di generazione 2

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>**Problema**  
*Una o più macchine virtuali di seconda generazione hanno una porta seriale configurata.*  
  
## <a name="impact"></a>**Impatto**  
*Le prestazioni potrebbero essere influenzate dalle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Se questo comportamento è intenzionale, non è necessaria alcuna azione aggiuntiva. In caso contrario, provare a usare la console di gestione di Hyper-V o Windows PowerShell per rimuovere la stringa di connessione dalle porte seriali della macchina virtuale.*  
  


