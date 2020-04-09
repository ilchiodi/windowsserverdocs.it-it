---
title: Aggiunta e sviluppo di funzionalità
description: System Insights consente di aggiungere nuove funzionalità predittive a System Insights, senza richiedere aggiornamenti del sistema operativo. Ciò consente agli sviluppatori, inclusi Microsoft e terze parti, di creare e fornire nuove funzionalità a metà della versione per risolvere gli scenari a cui si è interessati. Le nuove funzionalità possono specificare dati personalizzati da raccogliere e analizzare e integrarsi anche con i piani di gestione di System Insights esistenti.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 0dd4e24197d5a8c438d70a849e435ce28792dfce
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858434"
---
# <a name="adding-and-developing-new-capabilities"></a>Aggiunta e sviluppo di nuove funzionalità

>Si applica a: Windows Server 2019

System Insights consente di aggiungere nuove funzionalità predittive a System Insights, senza richiedere aggiornamenti del sistema operativo. Ciò consente agli sviluppatori, inclusi Microsoft e terze parti, di creare e fornire nuove funzionalità a metà della versione per risolvere gli scenari a cui si è interessati. 

Tutte le nuove funzionalità possono essere integrate con ed estendere l'infrastruttura esistente di System Insights:

- Le nuove funzionalità possono **specificare qualsiasi contatore delle prestazioni o evento di sistema**, che verrà raccolto, salvato in locale e restituito alla funzionalità di analisi quando viene richiamata la funzionalità.  
- Le nuove funzionalità possono **sfruttare l'interfaccia di amministrazione di Windows e i piani di gestione di PowerShell esistenti**. Non solo le nuove funzionalità saranno individuabili in System Insights, ma potranno anche trarre vantaggio da pianificazioni personalizzate e azioni correttive. 

## <a name="manage-new-capabilities"></a>Gestisci nuove funzionalità
- [Informazioni](add-remove-update-capabilities.md) su come aggiungere, rimuovere e aggiornare le funzionalità usando PowerShell. 

## <a name="develop-a-capability"></a>Sviluppare una funzionalità
Usare le risorse seguenti per iniziare a scrivere le proprie funzionalità personalizzate:
- [Informazioni sulle origini](data-sources.md) dati che è possibile raccogliere.
- [Scaricare](https://www.nuget.org/packages/Microsoft.WindowsServer.SystemInsights/) il pacchetto NuGet di System Insights, che contiene le classi e le interfacce necessarie per scrivere una funzionalità.
- Per informazioni sulle classi e le interfacce di System Insights, [vedere](https://aka.ms/systeminsights-api) la documentazione dell'API. 
- [Usare](https://aka.ms/systeminsights-samplecapability) la funzionalità di esempio di System Insights per iniziare. In questo modo viene illustrato come registrare una funzionalità, specificare le origini dati da raccogliere e avviare l'analisi dei dati di sistema.

>[!NOTE]
>Si tratta di una funzionalità di versione non definitiva. È soggetta a modifiche, in quanto si aggiungono nuove funzionalità e si incorporano commenti e suggerimenti.

## <a name="see-also"></a>Vedere anche
Per altre informazioni su System Insights, usare le risorse seguenti:

- [Panoramica di System Insights](overview.md)
- [Descrizione delle funzionalità](understanding-capabilities.md)
- [Gestione delle funzionalità](managing-capabilities.md)
- [Aggiunta, rimozione e aggiornamento di funzionalità](add-remove-update-capabilities.md)
- [Domande frequenti su System Insights](faq.md)