---
title: Le voci di autorizzazione devono avere nomi di gruppi di attendibilità distinti per i server primari con macchine virtuali che non fanno parte dello stesso gruppo di trust
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 22e1a3bdeb40c5440862b4931dda344756cc34a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857814"
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
*Potrebbero sussistere problemi di protezione e privacy con una macchina virtuale che accetta la replica dai server primari appartenenti a diverse voci di autorizzazione. Ciò influisca sulle seguenti voci di autorizzazione: \<elenco di voci di autorizzazione >*  
  
## <a name="resolution"></a>**Soluzione**  
*Utilizzare tag diversi nelle voci di autorizzazione per i server primari con macchine virtuali che non fanno parte dello stesso gruppo di sicurezza. Modificare le impostazioni di Hyper-V per configurare i tag di replica.*  
  


