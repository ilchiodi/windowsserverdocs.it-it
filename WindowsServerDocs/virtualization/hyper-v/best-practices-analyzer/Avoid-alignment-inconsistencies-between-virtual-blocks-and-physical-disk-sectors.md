---
title: Evitare le incoerenze di allineamento tra i blocchi virtuali e i settori del disco fisico in dischi rigidi virtuali dinamici o dischi differenze
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a17c8fd2-af81-485b-bfea-bd1ef3e43923
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 3f090f015f2179ba372e56d580477ef8d72d977f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857804"
---
# <a name="avoid-alignment-inconsistencies-between-virtual-blocks-and-physical-disk-sectors-on-dynamic-virtual-hard-disks-or-differencing-disks"></a>Evitare le incoerenze di allineamento tra i blocchi virtuali e i settori del disco fisico in dischi rigidi virtuali dinamici o dischi differenze

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
*Sono state rilevate incoerenze di allineamento per uno o più dischi rigidi virtuali.*  
  
### <a name="impact"></a>Impatto  
*Se i dischi rigidi virtuali vengono archiviati su disco fisico con dimensioni di settore pari a 4K, la macchina virtuale o le applicazioni che usano il disco rigido virtuale potrebbero riscontrare problemi di prestazioni. Ciò influiscono sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Utilizzare la creazione guidata disco rigido virtuale per creare un nuovo disco rigido virtuale in formato VHD o VHDX e specificare il disco rigido virtuale esistente come disco di origine. Il nuovo disco rigido virtuale verrà creato con l'allineamento tra i blocchi virtuali e il disco fisico.*  
  


