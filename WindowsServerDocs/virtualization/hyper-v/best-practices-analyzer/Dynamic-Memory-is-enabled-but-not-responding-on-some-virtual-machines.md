---
title: La memoria dinamica è abilitata ma non risponde in alcune macchine virtuali
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 91b7f50f-a071-4ab6-beb1-1b29f92f52b6
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 95fd426929f3e2f6f01bc10b207a21a57f1d8370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887782"
---
# <a name="dynamic-memory-is-enabled-but-not-responding-on-some-virtual-machines"></a>La memoria dinamica è abilitata ma non risponde in alcune macchine virtuali

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
*Uno o più macchine virtuali hanno problemi con il driver necessario per la memoria dinamica nel sistema operativo guest.*  
  
## <a name="impact"></a>Impatto  
*Il sistema operativo guest nelle macchine virtuali seguenti potrebbero non essere eseguiti o potrebbe essere unreliably perché Hyper-V non è possibile regolare la quantità di memoria in modo dinamico per rispondere alle modifiche nella richiesta di memoria. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Questo comportamento è previsto se è l'avvio della macchina virtuale. Se non si avvii la macchina virtuale, assicurarsi che i servizi di integrazione vengono aggiornati alla versione più recente e che il sistema operativo guest supporta la memoria dinamica.*  
  
A partire da Windows Server 2016, i servizi di integrazione vengono forniti tramite Windows Update. Verificare che le macchine virtuali sono configurate per ricevere gli aggiornamenti per ottenere la versione più recente di integration services.  
  
La memoria dinamica è compatibile con versioni specifiche di guest supportati. Vedere [Panoramica della memoria dinamica di Hyper-V](https://technet.microsoft.com/library/hh831766.aspx) per le versioni precedenti a Windows 10 e Windows Server 2016.  
  


