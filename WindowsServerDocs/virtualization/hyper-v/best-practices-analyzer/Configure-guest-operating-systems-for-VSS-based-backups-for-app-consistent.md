---
title: Configurare i sistemi operativi guest per i backup basati su VSS abilitare snapshot coerenti dell'applicazione per la Replica Hyper-V
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7638e996-d42d-47b8-a670-1e09e7183850
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b4300dd4b7adc0cef8544215b5da62044a97301b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863892"
---
# <a name="configure-guest-operating-systems-for-vss-based-backups-to-enable-application-consistent-snapshots-for-hyper-v-replica"></a>Configurare i sistemi operativi guest per i backup basati su VSS abilitare snapshot coerenti dell'applicazione per la Replica Hyper-V

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Errore|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Snapshot coerenti con l'applicazione richiede che servizi Copia Shadow del Volume (VSS) è abilitata e configurata nei sistemi operativi guest delle macchine virtuali che partecipano alla replica.*  
  
## <a name="impact"></a>Impatto  
*Anche se gli snapshot coerenti con l'applicazione vengono specificati nella configurazione di replica, Hyper-V non utilizzerà tali a meno che non è configurato VSS. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Per installare integration services nella macchina virtuale, usare connessione macchina virtuale.*  
  


