---
title: Evitare le incoerenze di allineamento tra blocchi virtuali e i settori del disco fisico su dischi rigidi virtuali dinamici o dischi differenze
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a17c8fd2-af81-485b-bfea-bd1ef3e43923
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4973c199a5507d00e15da8f621a09f0c602a29fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833862"
---
# <a name="avoid-alignment-inconsistencies-between-virtual-blocks-and-physical-disk-sectors-on-dynamic-virtual-hard-disks-or-differencing-disks"></a>Evitare le incoerenze di allineamento tra blocchi virtuali e i settori del disco fisico su dischi rigidi virtuali dinamici o dischi differenze

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
*Per uno o più dischi rigidi virtuali sono state rilevate incoerenze di allineamento.*  
  
### <a name="impact"></a>Impatto  
*Se i dischi rigidi virtuali vengono archiviati nel disco fisico con una dimensione di settore di 4K, la macchina virtuale o le applicazioni che utilizzano il disco rigido virtuale potrebbe riscontrare problemi di prestazioni. Ciò ha effetto sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Usare la creazione guidata disco rigido virtuale per creare un nuovo formato VHD o VHDX in formato disco rigido virtuale e specificare il disco rigido virtuale esistente come disco di origine. Verrà creato il nuovo disco rigido virtuale con l'allineamento tra i blocchi virtuali e disco fisico.*  
  


