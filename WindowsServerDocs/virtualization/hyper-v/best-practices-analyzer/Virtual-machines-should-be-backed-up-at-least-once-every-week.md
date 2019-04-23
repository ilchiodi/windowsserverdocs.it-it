---
title: Le macchine virtuali siano sottoposte a backup almeno una volta ogni settimana
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e079e3cb225ec9c712233bbf3efc85bb6f09b218
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826752"
---
# <a name="virtual-machines-should-be-backed-up-at-least-once-every-week"></a>Le macchine virtuali siano sottoposte a backup almeno una volta ogni settimana

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Errore|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Uno o più macchine virtuali è non stato eseguito il nella settimana precedente.*  
  
## <a name="impact"></a>Impatto  
*Se la macchina virtuale si verifica un problema e non è disponibile un backup recente, potrebbe verificarsi una perdita significativa di dati. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Pianifica un backup delle macchine virtuali da eseguire almeno una volta alla settimana. È possibile ignorare questa regola se la macchina virtuale è una replica e la macchina virtuale primaria viene eseguito il oppure se questa è la macchina virtuale primaria e la relativa replica durante il backup.*  
  


