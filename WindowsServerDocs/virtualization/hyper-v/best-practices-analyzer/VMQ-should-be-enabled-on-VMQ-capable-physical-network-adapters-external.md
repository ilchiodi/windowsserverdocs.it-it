---
title: VMQ deve essere abilitato in schede di rete fisiche con supporto per VMQ associato a un Commuter virtuale esterno
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: a66c1c4580f8ecda90caa5446e74bc9b12ac0476
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855044"
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
  
\<elenco di schede di rete >  
  
## <a name="resolution"></a>**Soluzione**  
*Abilitare VMQ con il cmdlet Enable-NetAdapterVmq di Windows PowerShell o usando l'interfaccia utente delle proprietà avanzate per la scheda di rete.*  
  


