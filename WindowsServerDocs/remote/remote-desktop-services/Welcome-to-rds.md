---
title: Servizi Desktop per remoto in Windows Server 2016
description: Viene fornita una panoramica di Servizi Desktop remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 02/22/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 52b9e09f-39e0-41a9-9d3b-4d5f4eacf3e0
author: christianmontoya
manager: scottman
ms.localizationpriority: medium
ms.openlocfilehash: cd00f92254f9e55f83442f5e68e344e0aa7579a2
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "1708530"
---
# <a name="welcome-to-remote-desktop-services"></a>Servizi Desktop remoto per 

Servizi Desktop remoto (RDS) è la piattaforma ideale per lo sviluppo di soluzioni di virtualizzazione per tutte le esigenze di analisi end, tra cui fornitura singole applicazioni virtualizzate, fornisce l'accesso mobile e remota desktop protetta e fornire agli utenti finali di possibilità di eseguire le applicazioni e desktop dal cloud.

![Cenni preliminari su Servizi Desktop remoto](.\media\rds-overview.png)

RDS offre la flessibilità di distribuzione, costo efficienza ed estendibilità, ovvero fruibile tramite un'ampia gamma di opzioni di distribuzione, tra cui Windows Server 2016 per le distribuzioni locali, Microsoft Azure per le distribuzioni cloud e un array affidabile di partner soluzioni.

In base all'ambiente e preferenze, è possibile configurare la soluzione di servizi desktop remoto per la virtualizzazione basata sulla sessione, come un'infrastruttura virtuale desktop (VDI) o una combinazione delle due configurazioni:

- **La virtualizzazione basata sulla sessione**: sfruttare la potenza di elaborazione di Windows Server per fornire un ambiente di sessione a più conveniente per guidare carichi di lavoro quotidiano degli utenti
- **VDI**: client Windows sfruttare fornire ad alte prestazioni, compatibilità delle applicazioni e conoscere i concetti che gli utenti aspettano dell'esperienza desktop di Windows.

In questi ambienti di virtualizzazione, è necessario maggiore flessibilità di pubblicare per gli utenti:

- **Desktop**: consentire agli utenti un'esperienza desktop completa con un'ampia gamma di installare e gestire le applicazioni. Ideale per gli utenti che si basano su questi computer come workstation principale o che sono provenienti da thin client, ad esempio con i servizi MultiPoint.
- **Su macchine virtuali**: specificare le singole applicazioni che sono ospitati ed esecuzione nella macchina virtuale, ma vengono visualizzati come se è in esecuzione sul desktop dell'utente come applicazioni locali. App contenere i propri voce sulla barra delle applicazioni e possono essere ridimensionate e spostate su monitor. Ideale per la distribuzione e gestione delle principali applicazioni nell'ambiente protetto in modalità remota consentendo agli utenti di lavorare da e personalizzare i propri desktop.

Per ambienti in cui è fondamentale convenienza e si desidera estendere i vantaggi della distribuzione desktop completo in un ambiente con la virtualizzazione basata sulla sessione, è possibile utilizzare [I servizi multipunto](../multipoint-services/multipoint-services.md) per fornire il valore ottimale. 

Con queste opzioni e le configurazioni, è necessario la flessibilità necessaria per distribuire le applicazioni che necessarie agli utenti in un remoto, sicuro e in modo economico e desktop.

## <a name="next-steps"></a>Passaggi successivi

Ecco alcuni passaggi successivi per ottenere una migliore comprensione di RDS e anche iniziare a distribuire il proprio ambiente:
-   Acquisire familiarità con le [configurazioni supportate](rds-supported-config.md) per RDS con diverse versioni di Windows e Windows Server
-   [Pianificare e progettare](rds-plan-and-design.md) un ambiente Servizi Desktop remoto per gestire requisiti diversi, ad esempio la disponibilità elevata e l'autenticazione a più fattori.
-   Esaminare i [modelli di architettura di Servizi Desktop remoto](desktop-hosting-logical-architecture.md) migliori per l'ambiente desiderato.
-   Iniziare a [distribuire l'ambiente di RDS con ARM e Azure Marketplace](rds-in-azure.md).
