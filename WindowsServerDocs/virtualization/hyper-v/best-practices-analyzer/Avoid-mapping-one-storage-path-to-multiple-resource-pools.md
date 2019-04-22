---
title: Evitare il mapping di un percorso di archiviazione a più pool di risorse
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 7c012836309f722e55c28b2ddbe3d54de641b4af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823962"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>Evitare il mapping di un percorso di archiviazione a più pool di risorse

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Operazioni|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.
  
## <a name="issue"></a>**Problema**  
*Viene eseguito il mapping di un percorso di file di archiviazione per più pool di risorse.*  
  
## <a name="impact"></a>**Impact**  
*Per il tipo di pool di archiviazione specificato, i seguenti pool padre e figlio condividono lo stesso percorso di archiviazione:*  
  
\<elenco di pool >  
  
## <a name="resolution"></a>**Soluzione**  
*Usare Windows PowerShell per riconfigurare i pool di risorse di archiviazione in modo che più pool di non usare lo stesso percorso di archiviazione.*  
  


