---
title: Configurare le impostazioni TCP/IP di Failover che si desidera che la macchina virtuale di Replica da utilizzare in caso di failover
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3f2681694d87b34369b29be6216ebec9210c6024
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366287"
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
*Clients con il carico di lavoro supportato dalla macchina virtuale primaria potrebbe non essere in grado di connettersi alla macchina virtuale di replica dopo un failover. Inoltre, indirizzo IP originale della macchina virtuale primaria non saranno valido nella topologia di rete macchina virtuale di Replica. Ciò influisca sulle macchine virtuali seguenti:*  
  
@no__t 0list di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Utilizzare la console di gestione di Hyper-V per configurare l'indirizzo IP che la macchina virtuale di replica deve utilizzare in caso di failover.*  
  


