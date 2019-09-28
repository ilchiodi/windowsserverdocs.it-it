---
title: La memoria dinamica è abilitata ma non risponde in alcune macchine virtuali
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 91b7f50f-a071-4ab6-beb1-1b29f92f52b6
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9aa482d91c94a7a619bb65046cf152d6a5f8827a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393684"
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
il sistema operativo guest *The nelle macchine virtuali seguenti potrebbe non essere eseguito o potrebbe non essere eseguito in modo inaffidabile perché Hyper-V non è in grado di regolare la memoria in modo dinamico per rispondere alle modifiche della richiesta di memoria. Ciò influisca sulle macchine virtuali seguenti:*  
  
@no__t 0list di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Stanziamento è un comportamento previsto se la macchina virtuale viene avviata. Se la macchina virtuale non viene avviata, assicurarsi che Integration Services venga aggiornato alla versione più recente e che il sistema operativo guest supporti memoria dinamica.*  
  
A partire da Windows Server 2016, i servizi di integrazione vengono forniti tramite Windows Update. Verificare che le macchine virtuali sono configurate per ricevere gli aggiornamenti per ottenere la versione più recente di integration services.  
  
La memoria dinamica è compatibile con versioni specifiche di guest supportati. Vedere [Panoramica della memoria dinamica di Hyper-V](https://technet.microsoft.com/library/hh831766.aspx) per le versioni precedenti a Windows 10 e Windows Server 2016.  
  


