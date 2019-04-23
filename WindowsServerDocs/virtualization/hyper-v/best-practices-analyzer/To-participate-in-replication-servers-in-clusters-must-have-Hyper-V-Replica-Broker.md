---
title: Per partecipare alla replica, server nei cluster di failover devono disporre di un gestore di Replica Hyper-V configurato
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5ec88ce5-a8b2-4ece-9062-366523c8b17f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: d4966396af955f9c8bad34b5b2892115e93c3b85
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887972"
---
# <a name="to-participate-in-replication-servers-in-failover-clusters-must-have-a-hyper-v-replica-broker-configured"></a>Per partecipare alla replica, server nei cluster di failover devono disporre di un gestore di Replica Hyper-V configurato

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
*Per i cluster di failover, la Replica Hyper-V richiede l'uso di un nome del gestore Replica Hyper-V anziché un nome di singoli server.*  
  
## <a name="impact"></a>Impatto  
*Se la macchina virtuale viene spostata in un nodo di cluster di failover diverso, la replica non può continuare.*  
  
## <a name="resolution"></a>Risoluzione  
*Utilizzare Gestione Cluster di Failover per configurare il gestore di Replica Hyper-V. Gestione di Hyper-V, assicurarsi che la configurazione della replica usi il nome del gestore Replica Hyper-V come nome del server.*  
  


