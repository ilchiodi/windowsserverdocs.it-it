---
title: Introduzione a Servizi Desktop remoto in Windows Server 2016
description: Panoramica di Servizi Desktop remoto
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 46a04905d5247ae940ca900297171d1112cf936b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404208"
---
# <a name="welcome-to-remote-desktop-services"></a>Introduzione a Servizi Desktop remoto 

Servizi Desktop remoto è la piattaforma ideale per la creazione di soluzioni di virtualizzazione per ogni esigenza dei clienti finali, tra cui la distribuzione di singole applicazioni virtualizzate, la disponibilità di un accesso desktop remoto e per dispositivi mobili sicuro e la possibilità, per gli utenti finali, di eseguire applicazioni e desktop dal cloud.

![Panoramica di Servizi Desktop remoto](./media/rds-overview.png)

Servizi Desktop remoto offre flessibilità di distribuzione, convenienza ed estendibilità con un'ampia gamma di opzioni di distribuzione, tra cui Windows Server 2016 per le distribuzioni locali, Microsoft Azure per le distribuzioni cloud e numerose soluzioni affidabili dei partner.

A seconda dell'ambiente e delle preferenze, puoi configurare la soluzione Servizi Desktop remoto per la virtualizzazione basata su sessione, come soluzione Virtual Desktop Infrastructure (VDI) o come una combinazione delle due opzioni:

- **Virtualizzazione basata su sessione**: sfrutta la potenza di calcolo di Windows Server per offrire un ambiente multisessione conveniente in modo da guidare i carichi di lavoro quotidiani degli utenti.
- **VDI**: sfrutta il client Windows per offrire compatibilità tra app, prestazioni elevate e la familiarità che gli utenti si aspettano dall'esperienza desktop in Windows.

In questi ambienti di virtualizzazione hai l'ulteriore flessibilità di scegliere cosa pubblicare per gli utenti:

- **Desktop**: offri agli utenti un'esperienza desktop completa con un'ampia gamma di applicazioni installate e gestite da te. Questa opzione è ideale per gli utenti che usano questi computer come workstation principali o che provengono da thin client, come Servizi MultiPoint.
- **RemoteApp**: specifica singole applicazioni che sono ospitate/eseguite nella macchina virtuale, ma che vengono visualizzate come se fossero in esecuzione sul desktop dell'utente, in modo analogo alle applicazioni locali. Le app hanno la propria voce sulla barra delle applicazioni e possono essere ridimensionate e spostate su più monitor. Questa opzione è ideale per distribuire e gestire applicazioni chiave in un ambiente sicuro e remoto, consentendo agli utenti di lavorare dai propri desktop e apportare personalizzazioni.

Per gli ambienti in cui la convenienza è essenziale e vuoi estendere i vantaggi della distribuzione di desktop completi in un ambiente di virtualizzazione basata su sessione, puoi usare [Servizi MultiPoint](../multipoint-services/multipoint-services.md) per offrire la soluzione migliore. 

Con queste opzioni e configurazioni, hai la flessibilità di distribuire desktop e applicazioni necessari agli utenti in un ambiente remoto, sicuro e conveniente.

## <a name="next-steps"></a>Passaggi successivi

Ecco alcuni passaggi successivi per comprendere meglio Servizi Desktop remoto e iniziare a distribuire un ambiente personalizzato:
-   Scopri le [configurazioni supportate](rds-supported-config.md) per Servizi Desktop remoto con le diverse versioni di Windows e Windows Server
-   [Pianifica e progetta](rds-plan-and-design.md) un ambiente di Servizi Desktop remoto per soddisfare requisiti diversi, ad esempio la disponibilità elevata e l'autenticazione a più fattori.
-   Esamina i [modelli di architettura di Servizi Desktop remoto](desktop-hosting-logical-architecture.md) che funzionano meglio per l'ambiente desiderato.
-   Inizia a [distribuire un ambiente di Servizi Desktop remoto con Azure Resource Manager e Azure Marketplace](rds-in-azure.md).
