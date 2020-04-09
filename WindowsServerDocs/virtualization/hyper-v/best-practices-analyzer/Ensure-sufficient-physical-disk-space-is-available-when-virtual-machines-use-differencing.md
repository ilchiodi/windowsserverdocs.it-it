---
title: Verificare che sia disponibile spazio su disco sufficiente quando le macchine virtuali usano dischi rigidi virtuali differenze
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 71f99aab-f994-4022-9da0-d661965b95ac
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 52e09cfb8695389d2c37def2c39ff43b4091fb76
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861954"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-differencing-virtual-hard-disks"></a>Verificare che sia disponibile spazio su disco sufficiente quando le macchine virtuali usano dischi rigidi virtuali differenze

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
*Una o più macchine virtuali utilizzano dischi rigidi virtuali differenze.*  
  
## <a name="impact"></a>Impatto  
*Per i dischi rigidi virtuali differenze è necessario spazio disponibile sul volume di hosting, in modo che lo spazio possa essere allocato quando si verificano scritture nei dischi rigidi virtuali. Se lo spazio disponibile è esaurito, potrebbero essere interessate tutte le macchine virtuali che si basano sull'archiviazione fisica. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Monitorare lo spazio disponibile su disco per garantire che lo spazio disponibile sia sufficiente per l'espansione del disco rigido virtuale. Provare a unire i dischi rigidi virtuali differenze nell'elemento padre. Nella console di gestione di Hyper-V, controllare il disco differenze per determinare il disco rigido virtuale padre. Se si unisce un disco differenze a un disco padre condiviso da altri dischi differenze, tale azione danneggerà la relazione tra gli altri dischi differenze e il disco padre, rendendoli inutilizzabili. Dopo aver verificato che il disco rigido virtuale padre non è condiviso, è possibile utilizzare la modifica guidata disco per unire il disco differenze al disco rigido virtuale padre.*  
  


