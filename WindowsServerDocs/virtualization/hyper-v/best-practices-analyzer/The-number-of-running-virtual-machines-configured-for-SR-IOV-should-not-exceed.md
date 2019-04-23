---
title: Il numero di macchine virtuali configurate per SR-IOV non deve superare il numero di funzioni virtuali disponibile per le macchine virtuali
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e63df9283927437f9cfc62c052d83b07fe599b34
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829752"
---
# <a name="the-number-of-running-virtual-machines-configured-for-sr-iov-should-not-exceed-the-number-of-virtual-functions-available-to-the-virtual-machines"></a>Il numero di macchine virtuali configurate per SR-IOV non deve superare il numero di funzioni virtuali disponibile per le macchine virtuali

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
*Sono non disponibili funzioni virtuali sufficiente per il numero di macchine virtuali configurate per single-root i/o virtualization (SR-IOV).*  
  
## <a name="impact"></a>Impatto  
*Prestazioni di rete potrebbero non essere ottimale nelle macchine virtuali seguenti:*  
   
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*È consigliabile disabilitare SR-IOV in uno o più macchine virtuali che non necessitano di una funzione virtuale SR-IOV.*  
  


