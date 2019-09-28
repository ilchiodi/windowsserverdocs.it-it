---
title: Le voci di autorizzazione devono avere nomi di gruppi di attendibilità distinti per i server primari con macchine virtuali che non fanno parte dello stesso gruppo di trust
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ab71e0e6562b73d81b871914fd0e9e76570c518e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366544"
---
# <a name="authorization-entries-should-have-distinct-trust-group-names-for-primary-servers-with-virtual-machines-that-are-not-part-of-the-same-trust-group"></a>Le voci di autorizzazione devono avere nomi di gruppi di attendibilità distinti per i server primari con macchine virtuali che non fanno parte dello stesso gruppo di trust

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
*Il server accetterà le richieste di replica per la macchina virtuale di replica da qualsiasi server nell'elenco di autorizzazioni associato allo stesso tag di replica della macchina virtuale.*  
  
## <a name="impact"></a>**Impatto**  
*There potrebbe essere un problema di privacy e sicurezza con una macchina virtuale che accetta la replica dai server primari appartenenti a diverse voci di autorizzazione. Ciò influisca sulle seguenti voci di autorizzazione: \<list delle voci di autorizzazione >*  
  
## <a name="resolution"></a>**Soluzione**  
@no__t 0Use tag diversi nelle voci di autorizzazione per i server primari con macchine virtuali che non fanno parte dello stesso gruppo di sicurezza. Modificare le impostazioni di Hyper-V per configurare i tag di replica. *  
  


