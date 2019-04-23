---
title: Abilitare tutte le schede di rete virtuale configurate per una macchina virtuale
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: fcd350b7-4240-4359-aadd-93e7ac4d314e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: fbb1ef5283f6ccf8dfa355a09a86040be80f53e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844232"
---
# <a name="enable-all-virtual-network-adapters-configured-for-a-virtual-machine"></a>Abilitare tutte le schede di rete virtuale configurate per una macchina virtuale

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*È possibile disabilitare uno o più schede di rete in una macchina virtuale.*  
  
## <a name="impact"></a>Impatto  
  
*Le macchine virtuali seguenti non disponga della connettività di rete:*  
  
\<elenco di nomi delle macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
  
*Usare Gestione dispositivi nel sistema operativo guest per abilitare tutte le schede di rete virtuale. Se l'adapter non è necessario, utilizzare Hyper-V Manager per rimuoverlo dalla macchina virtuale.*  
  


