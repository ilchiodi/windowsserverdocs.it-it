---
title: Aggiunta, rimozione e aggiornamento di funzionalità
description: Sistema Insights consente di creare nuove funzionalità che consentono di sfruttare la raccolta dei dati esistenti e funzionalità di gestione. È importante avere anche il supporto della piattaforma per gestire l'aggiunta, rimozione e gli aggiornamenti di queste funzionalità. Questo argomento descrive la funzionalità di alto livello per aggiungere, rimuovere e aggiornare le funzionalità nel sistema di Insights.
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
ms.date: 7/31/2018
ms.openlocfilehash: 07fb036d1c4aa4a63107594ec1f81cb5be1c7724
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880162"
---
# <a name="adding-removing-and-updating-capabilities"></a>Aggiunta, rimozione e aggiornamento di funzionalità

>Si applica a: Windows Server 2019

Sistema Insights consente di creare nuove funzionalità che consentono di sfruttare la raccolta dei dati esistenti e funzionalità di gestione. Dopo aver create queste funzionalità, tuttavia, è ugualmente importante avere anche il supporto della piattaforma per gestire l'aggiunta, rimozione e gli aggiornamenti di queste funzionalità. 

Questo argomento descrive la funzionalità di alto livello per aggiungere, rimuovere e aggiornare le funzionalità nel sistema di Insights. 

## <a name="adding-a-capability"></a>Aggiunta di una funzionalità
Sistema Insights consente di aggiungere nuove funzionalità in qualsiasi momento usando il **Add-InsightsCapability** cmdlet. Il **Add-InsightsCapability** richiede di specificare un nome di funzionalità e la libreria di funzionalità. La libreria di funzionalità contiene la descrizione delle funzionalità, origini dati e per la logica di stima.

```PowerShell
Add-InsightsCapability -Name "Sample capability" -Library "C:\SampleCapability.dll"
```

Dopo aver aggiunto una funzionalità al sistema di Insights, è immediatamente possibile richiamare e gestire la funzionalità tramite PowerShell o Windows Admin Center. 

## <a name="updating-a-capability"></a>L'aggiornamento di una funzionalità
Sistema Insights consente inoltre di aggiornare una funzionalità tramite il **Update-InsightsCapability** cmdlet.

```PowerShell
Update-InsightsCapability -Name "Sample capability" -Library "C:\SampleCapabilityv2.dll"
```

L'aggiornamento di una funzionalità consente di specificare una nuova libreria di funzionalità, che consente di modificare la descrizione di funzionalità, le origini dati e la logica di stima associata a tale funzionalità. È importante una capacità di aggiornamento consente di mantenere tutte le configurazione e le informazioni cronologiche su tale funzionalità, tra cui pianificazioni personalizzate, azioni e risultati di stima cronologica. 

## <a name="removing-a-capability"></a>Rimozione di una funzionalità
È anche possibile rimuovere le funzionalità nel sistema Insights usando il **Remove-InsightsCapability** cmdlet. 

```PowerShell
Remove-InsightsCapability -Name "Sample capability" 
```
>[!NOTE]
>Il valore predefinito di funzionalità di previsione non può essere rimosso.

Rimozione di una funzionalità in modo permanente consente di eliminare la funzionalità e tutti i relativi informazioni, tra cui la pianificazione di eventuali azioni di correzione e i risultati di stima passati. 

>[!TIP]
>Prendere in considerazione la disabilitazione di una funzionalità, anziché la rimozione se è preoccupati della eliminazione permanente di tutte le informazioni associate la funzionalità. 

## <a name="see-also"></a>Vedere anche
Per altre informazioni su System Insights, usare le risorse seguenti:

- [Panoramica sistema Insights](overview.md)
- [Funzionalità di comprensione](understanding-capabilities.md)
- [La funzionalità di gestione](managing-capabilities.md)
- [Aggiunta e lo sviluppo di funzionalità](adding-and-developing-capabilities.md)
- [Sistema Insights domande frequenti](faq.md)