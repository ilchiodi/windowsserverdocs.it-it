---
title: Evitare di abilitare la qualità del servizio di archiviazione quando si usa un disco rigido virtuale differenze quando i dischi rigidi virtuali padre e figlio si trovano su volumi diversi
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 4190b164b473e54c7fcecd68ddf8f746cebcfdc9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857784"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>Evitare di abilitare la qualità del servizio di archiviazione quando si usa un disco rigido virtuale differenze quando i dischi rigidi virtuali padre e figlio si trovano su volumi diversi

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.
  
## <a name="issue"></a>**Problema**  
*Per un disco rigido virtuale differenze con i dischi rigidi virtuali padre e figlio su volumi diversi è abilitata la qualità del servizio di archiviazione.*  
  
## <a name="impact"></a>**Impatto**  
*Questa configurazione può comportare un comportamento imprevisto di qualità del servizio di archiviazione per il disco rigido virtuale differenze, nonché altri dischi rigidi virtuali nei volumi padre e figlio. Ciò influisca sui dischi rigidi virtuali seguenti:*  
  
\<elenco di dischi rigidi virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Disabilitare la qualità del servizio di archiviazione nei dischi rigidi virtuali a cui viene fatto riferimento oppure eseguire una migrazione dell'archiviazione per spostare il disco rigido virtuale padre e figlio nello stesso volume.*  
  


