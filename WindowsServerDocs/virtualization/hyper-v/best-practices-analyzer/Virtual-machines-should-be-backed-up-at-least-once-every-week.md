---
title: Le macchine virtuali siano sottoposte a backup almeno una volta ogni settimana
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ec11b067de2c9f8cbb3a17731caa0dc526bf54a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393232"
---
# <a name="virtual-machines-should-be-backed-up-at-least-once-every-week"></a>Le macchine virtuali siano sottoposte a backup almeno una volta ogni settimana

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Error|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Non è stato eseguito il backup di una o più macchine virtuali nell'ultima settimana.*  
  
## <a name="impact"></a>Impatto  
*È possibile che si verifichi una perdita di dati significativa se la macchina virtuale rileva un problema e non esiste un backup recente. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Pianificare un backup delle macchine virtuali per l'esecuzione almeno una volta alla settimana. È possibile ignorare questa regola se la macchina virtuale è una replica e la macchina virtuale primaria viene sottoposta a backup, oppure se si tratta della macchina virtuale primaria e della replica di cui viene eseguito il backup.*  
  


