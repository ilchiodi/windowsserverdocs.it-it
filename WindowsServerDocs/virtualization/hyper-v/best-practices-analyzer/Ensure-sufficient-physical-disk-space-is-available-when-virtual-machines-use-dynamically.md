---
title: Verificare che sia disponibile spazio su disco sufficiente quando le macchine virtuali utilizzano dischi rigidi virtuali a espansione dinamica
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9e3e3e64-4b3a-4b9d-acf1-e4df61a04f1e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 53302d9e8fc4f960f0a1744d4ebd274b936eac5f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861944"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-dynamically-expanding-virtual-hard-disks"></a>Verificare che sia disponibile spazio su disco sufficiente quando le macchine virtuali utilizzano dischi rigidi virtuali a espansione dinamica

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
*Una o più macchine virtuali utilizzano dischi rigidi virtuali a espansione dinamica.*  
  
## <a name="impact"></a>Impatto  
*Per i dischi rigidi virtuali a espansione dinamica è necessario spazio disponibile sul volume di hosting, in modo che lo spazio possa essere allocato quando si verificano scritture nei dischi rigidi virtuali. Se lo spazio disponibile è esaurito, potrebbero essere interessate tutte le macchine virtuali che si basano sull'archiviazione fisica. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Monitorare lo spazio disponibile su disco per garantire che lo spazio disponibile sia sufficiente per l'espansione. È consigliabile arrestare la macchina virtuale e utilizzare la modifica guidata disco nella console di gestione di Hyper-V per convertire ogni disco rigido virtuale a espansione dinamica per la macchina virtuale in un disco rigido virtuale a dimensione fissa.*  
  


