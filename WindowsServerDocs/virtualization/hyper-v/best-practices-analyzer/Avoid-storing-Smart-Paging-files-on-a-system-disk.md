---
title: Evitare di archiviare file di Paging intelligente su un disco del sistema
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9b57c9b8-76c5-43c7-bfa6-2c95b3cb6510
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6abc84b406de7e7c33628ccee4e3af706efe5c70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886172"
---
# <a name="avoid-storing-smart-paging-files-on-a-system-disk"></a>Evitare di archiviare file di Paging intelligente su un disco del sistema

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Operazioni|  
  
Nelle sezioni seguenti, corsivo indica il testo visualizzato nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*La configurazione della memoria per una o più macchine virtuali potrebbe richiedere l'uso del Paging intelligente, se la macchina virtuale viene riavviata e il percorso specificato per il file di Paging intelligente è il disco del sistema del server che esegue Hyper-V.*  
  
## <a name="impact"></a>Impatto  
*Utilizzo del disco del sistema di Paging intelligente può causare il server che esegue Hyper-V persistono. Ciò ha effetto sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Riconfigurare le macchine virtuali per archiviare i file di Paging intelligente su un disco non di sistema.*  
  


