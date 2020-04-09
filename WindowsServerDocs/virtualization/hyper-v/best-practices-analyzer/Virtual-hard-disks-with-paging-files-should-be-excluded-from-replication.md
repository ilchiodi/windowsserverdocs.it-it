---
title: Dischi rigidi virtuali con file di paging devono essere esclusi dalla replica
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 94e03cf9de3991d003fad9019b9af33fad2f6bae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855024"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>Dischi rigidi virtuali con file di paging devono essere esclusi dalla replica

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Informazioni|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*I file di paging devono essere esclusi dalla replica, ma non sono stati esclusi dischi.*  
  
## <a name="impact"></a>Impatto  
*I file di paging presentano un volume elevato di attività di input/output, che richiederà inutilmente risorse molto maggiori per partecipare alla replica. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Se non è già stato fatto, creare un disco rigido virtuale distinto per il file di paging di Windows. Se la replica iniziale è già stata completata, utilizzare la console di gestione di Hyper-V per rimuovere la replica. Quindi, configurare di nuovo la replica ed escludere il disco rigido virtuale con il file di paging dalla replica.*  
  


