---
title: La memoria dinamica è abilitata ma non risponde in alcune macchine virtuali
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 91b7f50f-a071-4ab6-beb1-1b29f92f52b6
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 98bf61e6e132db8c8a16bf719410aefb433a1d99
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861984"
---
# <a name="dynamic-memory-is-enabled-but-not-responding-on-some-virtual-machines"></a>La memoria dinamica è abilitata ma non risponde in alcune macchine virtuali

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
*Una o più macchine virtuali riscontrano problemi con il driver necessario per memoria dinamica nel sistema operativo guest.*  
  
## <a name="impact"></a>Impatto  
*Il sistema operativo guest nelle macchine virtuali seguenti potrebbe non essere eseguito o potrebbe non essere eseguito in modo affidabile perché Hyper-V non è in grado di regolare la memoria in modo dinamico per rispondere alle modifiche della richiesta di memoria. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Si tratta di un comportamento previsto se la macchina virtuale viene avviata. Se la macchina virtuale non viene avviata, assicurarsi che Integration Services venga aggiornato alla versione più recente e che il sistema operativo guest supporti memoria dinamica.*  
  
A partire da Windows Server 2016, i servizi di integrazione vengono forniti tramite Windows Update. Verificare che le macchine virtuali sono configurate per ricevere gli aggiornamenti per ottenere la versione più recente di integration services.  
  
La memoria dinamica è compatibile con versioni specifiche di guest supportati. Vedere [Panoramica della memoria dinamica di Hyper-V](https://technet.microsoft.com/library/hh831766.aspx) per le versioni precedenti a Windows 10 e Windows Server 2016.  
  


