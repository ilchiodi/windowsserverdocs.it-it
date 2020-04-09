---
title: Domande frequenti su System Insights
description: Domande frequenti su System Insights
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: 9d6ddd682def579796089266065be7d39ce361d1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856254"
---
# <a name="system-insights-faq"></a>Domande frequenti su System Insights

>Si applica a: Windows Server 2019

## <a name="how-can-you-use-system-insights-with-azure-monitor-or-system-center-operations-manager"></a>Come è possibile usare System Insights con monitoraggio di Azure o System Center Operations Manager?

[Monitoraggio di Azure](https://azure.microsoft.com/services/monitor/) e [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) forniscono informazioni operative tra le distribuzioni per semplificare la gestione dell'infrastruttura. System Insights, al contrario, è una funzionalità di Windows Server che introduce le funzionalità di analisi predittiva locale. Insieme, System Insights e monitoraggio di Azure o SCOM possono aiutare a esporre le stime in una popolazione di dispositivi:

 Monitoraggio di Azure o SCOM possono ricavare gli eventi creati da System Insights, perché System Insights restituisce il risultato di ogni stima nel registro eventi. Possono esporre queste stime specifiche del computer in una flotta di server Windows, consentendo di disporre di una visualizzazione unificata di tali stime in un gruppo di istanze del server. 
 
 Vedere il canale e gli ID evento per ogni stima [qui](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results).

## <a name="how-does-system-insights-relate-to-windows-ml"></a>In che modo System Insights è correlato a Windows ML?

[Windows ml](https://docs.microsoft.com/windows/uwp/machine-learning/) è una piattaforma che consente agli sviluppatori di importare e assegnare punteggi ai modelli di apprendimento automatico pre-addestrati nei dispositivi Windows. Questi modelli traggono vantaggio dall'accelerazione hardware e possono essere classificati localmente. 

System Insights è una funzionalità di Windows Server 2019 che offre funzionalità predittive locali insieme a un'esperienza di gestione completa, tra cui l'integrazione di PowerShell e l'interfaccia di amministrazione di Windows. 

## <a name="can-i-use-system-insights-for-my-cluster"></a>È possibile usare System Insights per il cluster? 

Sì. System Insights può essere eseguito in modo indipendente in ogni singolo nodo del cluster di failover e il comportamento predefinito di System Insights prevede l'utilizzo in archiviazione locale, volume, CPU e rete. **È anche possibile abilitare la previsione per l'archiviazione in cluster**, in modo che le funzionalità di previsione dell'archiviazione prevedino l'utilizzo per i volumi e l'archiviazione in cluster. 

È possibile gestire queste impostazioni nell'interfaccia di amministrazione di Windows o in PowerShell e informazioni più dettagliate su questa funzionalità sono disponibili [qui](https://blogs.technet.microsoft.com/filecab/2018/10/03/using-system-insights-to-forecast-clustered-storage-usage/).
 

## <a name="how-expensive-is-it-to-run-the-default-capabilities"></a>Quanto è costoso eseguire le funzionalità predefinite?

Ogni funzionalità predefinita è a basso costo per l'esecuzione. Ogni funzionalità richiederà più tempo per l'esecuzione quando si raccolgono più dati, ma in genere dovrebbero essere completati in pochi secondi. 

## <a name="see-also"></a>Vedere anche
Per altre informazioni su System Insights, usare le risorse seguenti:

- [Panoramica di System Insights](overview.md)
- [Descrizione delle funzionalità](understanding-capabilities.md)
- [Gestione delle funzionalità](managing-capabilities.md)
- [Aggiunta e sviluppo di funzionalità](adding-and-developing-capabilities.md)
