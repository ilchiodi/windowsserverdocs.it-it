---
title: Configurare le impostazioni TCP/IP di Failover che si desidera che la macchina virtuale di Replica da utilizzare in caso di failover
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: d2db5fdedbe2f19c01b7dd172f18b6fec969e828
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862054"
---
# <a name="configure-the-failover-tcpip-settings-that-you-want-the-replica-virtual-machine-to-use-in-the-event-of-a-failover"></a>Configurare le impostazioni TCP/IP di Failover che si desidera che la macchina virtuale di Replica da utilizzare in caso di failover

>Si applica a: Windows Server 2016
 
Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.
  
## <a name="issue"></a>Problema  
*Le macchine virtuali di replica configurate con un indirizzo IP statico devono essere configurate per usare un indirizzo IP diverso dalla controparte della macchina virtuale primaria in caso di failover.*  
  
## <a name="impact"></a>Impatto  
*I client che usano il carico di lavoro supportato dalla macchina virtuale primaria potrebbero non essere in grado di connettersi alla macchina virtuale di replica dopo un failover. Inoltre, l'indirizzo IP originale della macchina virtuale primaria non sarà valido nella topologia di rete della macchina virtuale di replica. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Utilizzare la console di gestione di Hyper-V per configurare l'indirizzo IP che la macchina virtuale di replica deve utilizzare in caso di failover.*  
  


