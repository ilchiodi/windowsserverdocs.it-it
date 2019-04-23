---
title: Guida agli scenari dei criteri DNS
description: Questo argomento fa parte del DNS criteri Scenario Guide per Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b3a1400a68e22fc4988c87c9222b66f718cf5fd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865642"
---
# <a name="dns-policy-scenario-guide"></a>Guida agli scenari dei criteri DNS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questa guida è destinata dagli amministratori DNS, rete e sistemi.  
  
Criteri DNS è una nuova funzionalità per DNS in Windows Server&reg; 2016. È possibile usare questa guida per imparare a usare i criteri DNS per controllare la modalità di elaborazione delle query di risoluzione dei nomi in base a diversi parametri definiti nei criteri di un server DNS.   
  
Questa guida contiene le informazioni di panoramica dei criteri DNS, nonché scenari di criteri DNS specifici che vengono forniscono le istruzioni su come configurare il comportamento di server DNS per raggiungere i propri obiettivi, tra cui gestione del traffico basato su posizione geografica per il database primario e nei server DNS secondari, disponibilità elevata delle applicazioni, DNS "split Brain" e altro ancora.  
  
Questa guida contiene le sezioni seguenti.  
  
- [Cenni preliminari sui criteri DNS](DNS-Policies-Overview.md)  
- [Usare i criteri DNS per posizione geografica basato su gestione traffico con i server primari](primary-geo-location.md)  
- [Usare i criteri DNS per posizione geografica basato su gestione traffico con distribuzioni primario secondario](primary-secondary-geo-location.md)  
- [Usare i criteri DNS per risposte DNS intelligente basate sull'ora del giorno](dns-tod-intelligent.md)
- [Server di App Cloud risposte DNS basate sull'ora del giorno con un'istanza di Azure](dns-tod-azure-cloud-app-server.md)
- [Usare i criteri DNS per la distribuzione DNS "split Brain"](split-brain-DNS-deployment.md)
- [Usare i criteri DNS per DNS "split Brain" in Active Directory](dns-sb-with-ad.md)
- [Usare i criteri DNS per l'applicazione di filtri alle query DNS](apply-filters-on-dns-queries.md)
- [Usare i criteri DNS per bilanciamento del carico dell'applicazione](app-lb.md)
- [Usare i criteri DNS per applicazione bilanciamento del carico con la consapevolezza della posizione geografica](app-lb-geo.md)

