---
title: Assicurarsi che il driver di funzione virtuale venga eseguita correttamente quando una macchina virtuale è configurata per utilizzare SR-IOV
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d21e4b93-29bf-423a-a635-71c6d48dc49e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 282187d4d5a1243a14c3a0bdaa3088fe1f134bef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861924"
---
# <a name="ensure-that-the-virtual-function-driver-operates-correctly-when-a-virtual-machine-is-configured-to-use-sr-iov"></a>Assicurarsi che il driver di funzione virtuale venga eseguita correttamente quando una macchina virtuale è configurata per utilizzare SR-IOV

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
*Il driver della funzione virtuale non funziona correttamente nel sistema operativo guest di una o più macchine virtuali.*  
  
## <a name="impact"></a>Impatto  
*Le prestazioni di rete non sono ottimali nelle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Nel sistema operativo guest eseguire le operazioni seguenti: verificare che siano installati i driver appropriati e che siano abilitati tutti i dispositivi di rete e verificare la presenza di errori o avvisi nel registro eventi.*  
  


