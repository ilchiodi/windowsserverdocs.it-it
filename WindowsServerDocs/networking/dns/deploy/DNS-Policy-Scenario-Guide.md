---
title: Guida agli scenari dei criteri DNS
description: Questo argomento fa parte della Guida allo scenario dei criteri DNS per Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2168bc6d2f2b3a5f365bb2738a15ce7f96408de2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317926"
---
# <a name="dns-policy-scenario-guide"></a>Guida agli scenari dei criteri DNS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questa guida è destinata agli amministratori DNS, di rete e di sistema.  
  
Criteri DNS è una nuova funzionalità per DNS in Windows Server&reg; 2016. È possibile usare questa guida per informazioni su come usare i criteri DNS per controllare il modo in cui un server DNS elabora le query di risoluzione dei nomi in base a parametri diversi definiti nei criteri.   
  
Questa guida contiene informazioni generali sui criteri DNS, oltre a scenari di criteri DNS specifici che forniscono istruzioni su come configurare il comportamento del server DNS per realizzare gli obiettivi, inclusa la gestione del traffico basata sulla posizione geografica per i database primari e server DNS secondari, disponibilità elevata dell'applicazione, DNS "Split Brain" e altro ancora.  
  
Questa guida contiene le sezioni seguenti.  
  
- [Panoramica sui criteri DNS](DNS-Policies-Overview.md)  
- [Usare i criteri DNS per la gestione del traffico basata sulla posizione geografica con i server primari](primary-geo-location.md)  
- [Usare i criteri DNS per la gestione del traffico basata sulla posizione geografica con distribuzioni primarie secondarie](primary-secondary-geo-location.md)  
- [Usare i criteri DNS per le risposte DNS intelligenti in base all'ora del giorno](dns-tod-intelligent.md)
- [Risposte DNS basate sull'ora del giorno con un server app cloud di Azure](dns-tod-azure-cloud-app-server.md)
- [Usare i criteri DNS per la distribuzione DNS split brain](split-brain-DNS-deployment.md)
- [Usare i criteri DNS per DNS split-brain in Active Directory](dns-sb-with-ad.md)
- [Usare i criteri DNS per l'applicazione di filtri alle query DNS](apply-filters-on-dns-queries.md)
- [Usare i criteri DNS per il bilanciamento del carico dell'applicazione](app-lb.md)
- [Usare i criteri DNS per il bilanciamento del carico dell'applicazione con la consapevolezza della posizione geografica](app-lb-geo.md)

