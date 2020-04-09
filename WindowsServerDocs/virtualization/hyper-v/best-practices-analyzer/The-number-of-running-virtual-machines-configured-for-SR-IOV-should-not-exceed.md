---
title: Il numero di macchine virtuali configurate per SR-IOV non deve superare il numero di funzioni virtuali disponibile per le macchine virtuali
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 3c58e980471284c950f41551b02b92bf74aca7cc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854614"
---
# <a name="the-number-of-running-virtual-machines-configured-for-sr-iov-should-not-exceed-the-number-of-virtual-functions-available-to-the-virtual-machines"></a>Il numero di macchine virtuali configurate per SR-IOV non deve superare il numero di funzioni virtuali disponibile per le macchine virtuali

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
*Non sono disponibili funzioni virtuali sufficienti per il numero di macchine virtuali in esecuzione configurate per Single-Root I/O Virtualization (SR-IOV).*  
  
## <a name="impact"></a>Impatto  
*Le prestazioni di rete potrebbero non essere ottimali nelle macchine virtuali seguenti:*  
   
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Provare a disabilitare SR-IOV in una o più macchine virtuali che non richiedono una funzione virtuale SR-IOV.*  
  


