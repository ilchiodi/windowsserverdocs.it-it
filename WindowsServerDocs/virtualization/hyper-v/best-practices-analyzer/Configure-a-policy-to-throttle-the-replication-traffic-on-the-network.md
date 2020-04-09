---
title: Configurare un criterio per limitare il traffico di replica sulla rete
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 82cb1aef-cdc3-4d0a-88d4-ef497ab79606
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 2c358cd930f2b95412b40aa6c87b0bf9ebb5b741
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862174"
---
# <a name="configure-a-policy-to-throttle-the-replication-traffic-on-the-network"></a>Configurare un criterio per limitare il traffico di replica sulla rete

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
*Potrebbe non essere previsto un limite alla quantità di larghezza di banda di rete che la replica può utilizzare.*  
  
## <a name="impact"></a>Impatto  
*La larghezza di banda di rete può diventare completamente dominata dal traffico di replica, che influisce su altre attività di rete critiche. Ciò influisca sulle porte seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Se si usa un altro metodo per limitare il traffico di rete, è possibile ignorare questo problema. In caso contrario, usare Criteri di gruppo editor per configurare un criterio che limiterà il traffico di rete alla porta pertinente del server di replica.*  
  
  


