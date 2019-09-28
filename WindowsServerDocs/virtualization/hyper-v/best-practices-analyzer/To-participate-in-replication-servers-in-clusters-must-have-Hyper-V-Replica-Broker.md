---
title: Per partecipare alla replica, server nei cluster di failover devono disporre di un gestore di Replica Hyper-V configurato
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5ec88ce5-a8b2-4ece-9062-366523c8b17f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2e15d2c4a467807397ef4712d2df1730b40d8024
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364602"
---
# <a name="to-participate-in-replication-servers-in-failover-clusters-must-have-a-hyper-v-replica-broker-configured"></a>Per partecipare alla replica, server nei cluster di failover devono disporre di un gestore di Replica Hyper-V configurato

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Errore|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Per i cluster di failover, la replica Hyper-V richiede l'uso di un nome del gestore di replica Hyper-V anziché un singolo nome di server.*  
  
## <a name="impact"></a>Impatto  
*Se la macchina virtuale viene spostata in un altro nodo del cluster di failover, la replica non potrà continuare.*  
  
## <a name="resolution"></a>Risoluzione  
*Use Gestione cluster di failover per configurare il gestore di replica Hyper-V. Nella console di gestione di Hyper-V verificare che la configurazione della replica usi il nome del gestore di replica Hyper-V come nome del server.*  
  


