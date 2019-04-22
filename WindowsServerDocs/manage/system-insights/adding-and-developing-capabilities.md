---
title: Aggiunta e sviluppo di funzionalità
description: Sistema Insights consente di aggiungere nuove capacità predittiva Insights System, senza richiedere eventuali aggiornamenti del sistema operativo. Ciò consente agli sviluppatori, tra cui Microsoft e terze parti, per creare e distribuire una nuova versione a metà di funzionalità per soddisfare gli scenari in cui che si è interessati. Nuove funzionalità è possibile specificare dati personalizzati per raccogliere e analizzare e si integrano inoltre con i piani di gestione di System Insights esistenti.
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
ms.openlocfilehash: 8caddead774ac69a38906f3c0a0d2eaf005c1d28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817482"
---
# <a name="adding-and-developing-new-capabilities"></a>Aggiunta e lo sviluppo di nuove funzionalità

>Si applica a: Windows Server 2019

Sistema Insights consente di aggiungere nuove capacità predittiva Insights System, senza richiedere eventuali aggiornamenti del sistema operativo. Ciò consente agli sviluppatori, tra cui Microsoft e terze parti, per creare e distribuire una nuova versione a metà di funzionalità per soddisfare gli scenari in cui che si è interessati. 

Qualsiasi nuova funzionalità può integrare ed estendere l'infrastruttura di System Insights esistente:

- Nuove funzionalità possono **specificare qualsiasi evento di sistema o del contatore delle prestazioni**, che verrà raccolti, mantenuto in locale e restituita per la funzionalità per l'analisi quando la funzionalità viene richiamata.  
- Nuove funzionalità possono **sfruttare i vantaggi di Windows Admin Center esistente e piani di gestione di PowerShell**. Non solo sarà nuove funzionalità di essere individuabile nel sistema di Insights, cui trarre vantaggio dalle pianificazioni personalizzate e le azioni correttive. 

## <a name="manage-new-capabilities"></a>Gestire le nuove funzionalità
- [Informazioni su](add-remove-update-capabilities.md) come aggiungere, rimuovere e aggiornare le funzionalità usando PowerShell. 

## <a name="develop-a-capability"></a>Sviluppare una funzionalità
Usare le risorse seguenti che consentono di iniziare a scrivere il proprio funzionalità personalizzato:
- [Informazioni su](data-sources.md) sulle origini dati è possibile raccogliere.
- [Scaricare](https://www.nuget.org/packages/Microsoft.WindowsServer.SystemInsights/) il pacchetto NuGet di Insights di sistema, che contiene le classi e interfacce che è necessario scrivere una funzionalità.
- [Visitare](https://aka.ms/systeminsights-api) la documentazione dell'API per apprendere le interfacce e classi di informazioni di sistema. 
- [Usare](https://aka.ms/systeminsights-samplecapability) la funzionalità di esempio Insights di sistema che consentono di iniziare. Ciò viene illustrato come registrare una funzionalità, specificare le origini dati da raccogliere e iniziare ad analizzare i dati di sistema.

>[!NOTE]
>Si tratta di funzionalità della versione provvisoria. È soggetto a modifiche, aggiungere nuove funzionalità e includere il feedback.

## <a name="see-also"></a>Vedere anche
Per altre informazioni su System Insights, usare le risorse seguenti:

- [Panoramica sistema Insights](overview.md)
- [Funzionalità di comprensione](understanding-capabilities.md)
- [La funzionalità di gestione](managing-capabilities.md)
- [Aggiunta, rimozione e aggiornamento delle funzionalità](add-remove-update-capabilities.md)
- [Sistema Insights domande frequenti](faq.md)