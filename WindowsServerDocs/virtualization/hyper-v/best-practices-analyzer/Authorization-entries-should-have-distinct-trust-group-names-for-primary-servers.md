---
title: Le voci di autorizzazione devono avere nomi di gruppo di trust distinti per i server primari con le macchine virtuali che non fanno parte dello stesso gruppo di trust
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e5c5b1a6bf8ef0bbceb5dde6b28cd951f399fc5e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882962"
---
# <a name="authorization-entries-should-have-distinct-trust-group-names-for-primary-servers-with-virtual-machines-that-are-not-part-of-the-same-trust-group"></a>Le voci di autorizzazione devono avere nomi di gruppo di trust distinti per i server primari con le macchine virtuali che non fanno parte dello stesso gruppo di trust

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
*Il server accetterà le richieste di replica per la macchina virtuale di replica da qualsiasi server nell'elenco di autorizzazioni associate con lo stesso tag di replica della macchina virtuale.*  
  
## <a name="impact"></a>**Impact**  
*Potrebbero esserci problemi di sicurezza con una macchina virtuale accetta la replica da server primari che appartengono alle voci di autorizzazione diversi e privacy. Questo influisce sulle seguenti voci di autorizzazione: \<elenco di voci di autorizzazione >*  
  
## <a name="resolution"></a>**Soluzione**  
*Usare tag diversi nelle voci di autorizzazione per i server primari con le macchine virtuali che non fanno parte dello stesso gruppo di sicurezza. Modificare le impostazioni di Hyper-V per configurare i tag di replica.*  
  


