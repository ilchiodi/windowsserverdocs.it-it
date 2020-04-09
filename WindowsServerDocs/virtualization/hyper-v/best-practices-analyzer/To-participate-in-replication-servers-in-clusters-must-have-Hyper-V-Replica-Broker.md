---
title: Per partecipare alla replica, server nei cluster di failover devono disporre di un gestore di Replica Hyper-V configurato
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5ec88ce5-a8b2-4ece-9062-366523c8b17f
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 921d31aa63bcaaf0946c487d327144f5e29bcfe0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854584"
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
*Usare Gestione cluster di failover per configurare il gestore di replica Hyper-V. Nella console di gestione di Hyper-V verificare che la configurazione della replica usi il nome del gestore di replica Hyper-V come nome del server.*  
  


