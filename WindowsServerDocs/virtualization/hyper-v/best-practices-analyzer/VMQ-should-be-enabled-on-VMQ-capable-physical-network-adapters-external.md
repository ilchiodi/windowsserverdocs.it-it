---
title: Coda macchine Virtuali devono essere abilitata nelle schede di rete fisica con supporto per coda macchine Virtuali associate a un commutatore virtuale esterno
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9a552c15675e6ca7a7310c8c9eaec883653987be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889472"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>Coda macchine Virtuali devono essere abilitata nelle schede di rete fisica con supporto per coda macchine Virtuali associate a un commutatore virtuale esterno

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>**Problema**  
*Le schede di rete seguenti sono in grado di coda macchine virtuali (VMQ) ma la funzionalità è disabilitata.*  
  
## <a name="impact"></a>**Impact**  
*Windows è in grado di sfruttare appieno gli offload hardware disponibili nelle schede di rete seguenti:*  
  
\<elenco di schede di rete >  
  
## <a name="resolution"></a>**Soluzione**  
*Abilita coda macchine Virtuali usando il cmdlet Enable-NetAdapterVmq Windows PowerShell o tramite l'interfaccia utente delle proprietà avanzate per la scheda di rete.*  
  


