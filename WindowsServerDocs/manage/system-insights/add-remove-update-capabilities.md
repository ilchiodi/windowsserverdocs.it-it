---
title: Aggiunta, rimozione e aggiornamento di funzionalità
description: System Insights consente di creare nuove funzionalità che sfruttano le funzionalità di gestione e raccolta dati esistenti. È importante avere anche il supporto della piattaforma per gestire l'aggiunta, la rimozione e gli aggiornamenti di queste funzionalità. Questo argomento descrive le funzionalità di alto livello per aggiungere, rimuovere e aggiornare le funzionalità in System Insights.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 0a760f25de79bc89b2aa67aec6bb1e3a493c1310
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856264"
---
# <a name="adding-removing-and-updating-capabilities"></a>Aggiunta, rimozione e aggiornamento di funzionalità

>Si applica a: Windows Server 2019

System Insights consente di creare nuove funzionalità che sfruttano le funzionalità di gestione e raccolta dati esistenti. Una volta create queste funzionalità, tuttavia, è ugualmente importante avere anche il supporto della piattaforma per gestire l'aggiunta, la rimozione e gli aggiornamenti di queste funzionalità. 

Questo argomento descrive le funzionalità di alto livello per aggiungere, rimuovere e aggiornare le funzionalità in System Insights. 

## <a name="adding-a-capability"></a>Aggiunta di una funzionalità
System Insights consente di aggiungere nuove funzionalità in qualsiasi momento usando il cmdlet **Add-InsightsCapability** . Per **Add-InsightsCapability** è necessario specificare un nome di funzionalità e la libreria di funzionalità. La libreria di funzionalità contiene la descrizione della funzionalità, le origini dati e la logica di stima.

```PowerShell
Add-InsightsCapability -Name Sample capability -Library C:\SampleCapability.dll
```

Dopo che una funzionalità è stata aggiunta a System Insights, è possibile richiamare e gestire immediatamente la funzionalità usando PowerShell o l'interfaccia di amministrazione di Windows. 

## <a name="updating-a-capability"></a>Aggiornamento di una funzionalità
System Insights consente inoltre di aggiornare una funzionalità tramite il cmdlet **Update-InsightsCapability** .

```PowerShell
Update-InsightsCapability -Name Sample capability -Library C:\SampleCapabilityv2.dll
```

L'aggiornamento di una funzionalità consente di specificare una nuova libreria di funzionalità, che consente di modificare la descrizione della funzionalità, le origini dati e la logica di stima associata a tale funzionalità. In particolare, l'aggiornamento di una funzionalità consente di mantenere tutte le informazioni sulla configurazione e sulla cronologia di tale funzionalità, incluse le pianificazioni personalizzate, le azioni e i risultati della stima cronologica. 

## <a name="removing-a-capability"></a>Rimozione di una funzionalità
È anche possibile rimuovere le funzionalità in System Insights tramite il cmdlet **Remove-InsightsCapability** . 

```PowerShell
Remove-InsightsCapability -Name Sample capability 
```
>[!NOTE]
>Non è possibile rimuovere le funzionalità di previsione predefinite.

La rimozione di una funzionalità consente di eliminare definitivamente la funzionalità e tutte le informazioni associate, incluse la pianificazione, eventuali azioni correttive e i risultati della stima precedenti. 

>[!TIP]
>Provare a disabilitare una funzionalità anziché rimuoverla se si è preoccupati di eliminare definitivamente tutte le informazioni associate alla funzionalità. 

## <a name="see-also"></a>Vedere anche
Per altre informazioni su System Insights, usare le risorse seguenti:

- [Panoramica di System Insights](overview.md)
- [Descrizione delle funzionalità](understanding-capabilities.md)
- [Gestione delle funzionalità](managing-capabilities.md)
- [Aggiunta e sviluppo di funzionalità](adding-and-developing-capabilities.md)
- [Domande frequenti su System Insights](faq.md)