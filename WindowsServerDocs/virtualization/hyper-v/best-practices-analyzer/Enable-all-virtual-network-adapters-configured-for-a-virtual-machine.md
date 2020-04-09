---
title: Abilitare tutte le schede di rete virtuale configurate per una macchina virtuale
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: fcd350b7-4240-4359-aadd-93e7ac4d314e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 79971ff503afcc1a087aced579e30989233c2b4a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861964"
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
  
\<elenco dei nomi delle macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
  
*Utilizzare Device Manager nel sistema operativo guest per abilitare tutte le schede di rete virtuali. Se l'adapter non è necessario, utilizzare la console di gestione di Hyper-V per rimuoverlo dalla macchina virtuale.*  
  


