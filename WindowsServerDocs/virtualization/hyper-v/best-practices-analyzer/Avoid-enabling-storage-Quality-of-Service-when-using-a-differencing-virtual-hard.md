---
title: Evitare di abilitare QoS di archiviazione quando si usa un disco rigido virtuale differenze quando i dischi rigidi virtuali padre e figlio sono in volumi diversi
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2bdc8462c4d9dc50dbb69792f2f294add0ca3a74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856202"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>Evitare di abilitare QoS di archiviazione quando si usa un disco rigido virtuale differenze quando i dischi rigidi virtuali padre e figlio sono in volumi diversi

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.
  
## <a name="issue"></a>**Problema**  
*Storage Quality of Service abilitato dispone di un disco rigido virtuale differenze con i dischi rigidi virtuali padre e figlio su volumi diversi.*  
  
## <a name="impact"></a>**Impact**  
*Questa configurazione può comportare archiviazione imprevisto problema di qualità del servizio per il disco rigido virtuale differenze, nonché altri dischi rigidi virtuali nei volumi padre e figlio. Questo influisce sulle seguenti i dischi rigidi virtuali:*  
  
\<elenco dei dischi rigidi virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Disabilitare storage Quality of Service in cui viene fatto riferimento dischi rigidi virtuali, o eseguire una migrazione di archiviazione per spostare l'elemento padre e il disco rigido virtuale figlio allo stesso volume.*  
  


