---
title: Abilitare tutte le schede di rete virtuale configurate per una macchina virtuale
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: fcd350b7-4240-4359-aadd-93e7ac4d314e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: bdca25be4af41d0f6ddfafe885f8c2b1301b71fb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393653"
---
# <a name="enable-all-virtual-network-adapters-configured-for-a-virtual-machine"></a>Abilitare tutte le schede di rete virtuale configurate per una macchina virtuale

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Una o più schede di rete possono essere disabilitate in una macchina virtuale.*  
  
## <a name="impact"></a>Impatto  
  
*Le macchine virtuali seguenti potrebbero non avere connettività di rete:*  
  
@no__t 0list di nomi di macchina virtuale >  
  
## <a name="resolution"></a>Risoluzione  
  
*Use Device Manager nel sistema operativo guest per abilitare tutte le schede di rete virtuali. Se l'adapter non è necessario, utilizzare la console di gestione di Hyper-V per rimuoverlo dalla macchina virtuale.*  
  


