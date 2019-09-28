---
title: Evitare di archiviare file di paging intelligenti in un disco di sistema
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9b57c9b8-76c5-43c7-bfa6-2c95b3cb6510
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e3ddb662d14545693e26eb680527d93eb65d5d13
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365243"
---
# <a name="avoid-storing-smart-paging-files-on-a-system-disk"></a>Evitare di archiviare file di paging intelligenti in un disco di sistema

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Operazioni|  
  
Nelle sezioni seguenti, corsivo indica il testo visualizzato nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*La configurazione della memoria per una o più macchine virtuali potrebbe richiedere l'uso del paging intelligente se la macchina virtuale viene riavviata e il percorso specificato per il file di paging intelligente è il disco di sistema del server che esegue Hyper-V.*  
  
## <a name="impact"></a>Impatto  
@no__t 0Use del disco di sistema per il paging intelligente potrebbe causare problemi al server che esegue Hyper-V. Ciò influiscono sulle macchine virtuali seguenti: *  
  
@no__t 0list di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Riconfigurare le macchine virtuali per archiviare i file di paging intelligente in un disco non di sistema.*  
  


