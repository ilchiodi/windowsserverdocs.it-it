---
title: Evitare di abilitare la qualità del servizio di archiviazione quando si usa un disco rigido virtuale differenze quando i dischi rigidi virtuali padre e figlio si trovano su volumi diversi
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 716a32de2f9327e5eca38c470fa1b7c44150e9cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366445"
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
la configurazione *Stanziamento può comportare un comportamento imprevisto di qualità del servizio di archiviazione per il disco rigido virtuale differenze, oltre ad altri dischi rigidi virtuali nei volumi padre e figlio. Ciò influisca sui dischi rigidi virtuali seguenti:*  
  
@no__t 0list di dischi rigidi virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Disabilitare la qualità del servizio di archiviazione nei dischi rigidi virtuali a cui viene fatto riferimento oppure eseguire una migrazione dell'archiviazione per spostare il disco rigido virtuale padre e figlio nello stesso volume.*  
  


