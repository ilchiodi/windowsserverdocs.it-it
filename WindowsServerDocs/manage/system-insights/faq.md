---
title: Sistema Insights domande frequenti
description: Sistema Insights domande frequenti
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: system-insights
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: 13767e1336d1ff729d1fbbe6cae3ed57d68cefc4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851062"
---
# <a name="system-insights-faq"></a>Sistema Insights domande frequenti

>Si applica a: Windows Server 2019

## <a name="how-can-you-use-system-insights-with-azure-monitor-or-system-center-operations-manager"></a>Come si può utilizzare System Insights con monitoraggio di Azure o System Center Operations Manager?

[Monitoraggio di Azure](https://azure.microsoft.com/services/monitor/) e [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) forniscono informazioni operative tra le distribuzioni per semplificare la gestione dell'infrastruttura. Informazioni sistema, invece, è una funzionalità di Windows Server che introduce funzionalità di analitica predittiva locale. Insieme, sistema Insights e monitoraggio di Azure o SCOM possono essere utili le stime della superficie di attacco in una popolazione dei dispositivi:

 Monitoraggio di Azure o SCOM possono chiave disattivare gli eventi creati dal sistema Insights, come sistema Insights restituisce il risultato di ogni stima nel registro eventi. In una serie di Windows Server, consentendo di avere una visione generale di queste stime su un gruppo di istanze del server, fanno emergere queste stime specifiche del computer. 
 
 Visualizzare gli ID evento e del canale per ogni stima [qui](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results).

## <a name="how-does-system-insights-relate-to-windows-ml"></a>In che modo Insights sistema correlato a Machine Learning Windows?

[Machine Learning Windows](https://docs.microsoft.com/windows/uwp/machine-learning/) è una piattaforma che consente agli sviluppatori di importare e assegnare punteggi a modelli di apprendimento automatico con training preliminare su dispositivi Windows. Questi modelli di trarre vantaggio dall'accelerazione hardware, e possono essere classificati in locale. 

Sistema Insights è una funzionalità di Windows Server 2019 che offre funzionalità predittive locale insieme a un'esperienza di gestione completa, inclusa l'integrazione di PowerShell e Windows Admin Center. 

## <a name="can-i-use-system-insights-for-my-cluster"></a>È possibile usare informazioni dettagliate di sistema per il cluster? 

Sì. Sistema Insights può eseguire in modo indipendente su ogni singolo nodo del cluster e il comportamento predefinito dell'utilizzo delle previsioni di Insights sistema tra archiviazione locale, volume, CPU e rete. **È anche possibile abilitare le previsioni per l'archiviazione cluster**, in modo che le funzionalità di previsione archiviazione stima dell'utilizzo per l'archiviazione e i volumi del cluster. 

È possibile gestire queste impostazioni in Windows Admin Center o PowerShell, e informazioni più dettagliate su questa funzionalità sono disponibile [qui](https://blogs.technet.microsoft.com/filecab/2018/10/03/using-system-insights-to-forecast-clustered-storage-usage/).
 

## <a name="how-expensive-is-it-to-run-the-default-capabilities"></a>Costo richiesto è possibile eseguire le funzionalità predefinite?

Ogni funzionalità predefinita è conveniente per l'esecuzione. Ogni funzionalità richiederà più tempo per l'esecuzione come si raccolgono più dati, ma sono in genere completata entro un solo pochi secondi. 

## <a name="see-also"></a>Vedere anche
Per altre informazioni su System Insights, usare le risorse seguenti:

- [Panoramica sistema Insights](overview.md)
- [Funzionalità di comprensione](understanding-capabilities.md)
- [La funzionalità di gestione](managing-capabilities.md)
- [Aggiunta e lo sviluppo di funzionalità](adding-and-developing-capabilities.md)
