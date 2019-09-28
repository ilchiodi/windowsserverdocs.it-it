---
title: VMQ deve essere abilitato in schede di rete fisiche con supporto per VMQ associato a un Commuter virtuale esterno
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e8607ee891ef693d4e4e7a868540237855aebd2e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393294"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>VMQ deve essere abilitato in schede di rete fisiche con supporto per VMQ associato a un Commuter virtuale esterno

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>**Problema**  
*Le seguenti schede di rete sono in grado di supportare la coda delle macchine virtuali (VMQ), ma la funzionalità è disabilitata.*  
  
## <a name="impact"></a>**Impatto**  
*Windows non è in grado di sfruttare tutti i vantaggi degli offload hardware disponibili sulle schede di rete seguenti:*  
  
@no__t 0list di schede di rete >  
  
## <a name="resolution"></a>**Soluzione**  
*Abilitare VMQ con il cmdlet Enable-NetAdapterVmq di Windows PowerShell o usando l'interfaccia utente delle proprietà avanzate per la scheda di rete.*  
  


