---
title: Configurare le impostazioni TCP/IP di Failover che si desidera che la macchina virtuale di Replica da utilizzare in caso di failover
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c16fbc95c9d679611d57327992a6621d58d4e201
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855752"
---
# <a name="configure-the-failover-tcpip-settings-that-you-want-the-replica-virtual-machine-to-use-in-the-event-of-a-failover"></a>Configurare le impostazioni TCP/IP di Failover che si desidera che la macchina virtuale di Replica da utilizzare in caso di failover

>Si applica a: Windows Server 2016
 
Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.
  
## <a name="issue"></a>Problema  
*Macchine virtuali di replica configurate con un indirizzo IP statico deve essere configurate per usare un indirizzo IP diverso da loro controparte di macchina virtuale primaria nel caso di failover.*  
  
## <a name="impact"></a>Impatto  
*I client che usano il carico di lavoro supportato dalla macchina virtuale primaria potrebbero non essere in grado di connettersi alla macchina virtuale di Replica dopo un failover. Inoltre, indirizzo IP originale della macchina virtuale primaria non saranno valido nella topologia di rete macchina virtuale di Replica. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Utilizzare Hyper-V Manager per configurare l'indirizzo IP che la macchina virtuale di Replica deve usare nel caso di failover.*  
  


