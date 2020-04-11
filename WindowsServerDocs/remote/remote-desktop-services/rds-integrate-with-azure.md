---
title: Servizi Desktop remoto - Integrazione con i servizi di Azure
description: Informazioni su come integrare Servizi Desktop remoto nella distribuzione di Azure e Azure nella distribuzione di Servizi Desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/18/2017
ms.topic: article
author: lizap
ms.openlocfilehash: 2c792a22608fe0e7ae826b27d6917202037559e4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855074"
---
# <a name="remote-desktop-services---integrating-with-azure-services"></a>Servizi Desktop remoto - Integrazione con i servizi di Azure

Windows Server 2016 combina le potenti funzionalità di distribuzione sicura di desktop e app tramite Servizi Desktop remoto con i servizi flessibili e scalabili forniti da Microsoft Azure. Puoi distribuire Servizi Desktop remoto con i servizi di Azure per ridurre i costi di manutenzione dell'infrastruttura per i server locali, aumentare la stabilità usando i servizi di Azure per garantire la disponibilità elevata, migliorare la sicurezza tramite Multi-Factor Authentication e migliorare l'esperienza degli utenti grazie all'uso delle identità esistenti per l'accesso alle risorse in Servizi Desktop remoto.

Usa le informazioni seguenti per integrare Azure nella distribuzione di Desktop remoto:

- [Scoprire come usare l'autenticazione a più fattori con Servizi Desktop remoto](/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway)
- [Integrare Azure AD Domain Services con la distribuzione di Servizi Desktop remoto](rds-azure-adds.md)
- [Pubblicare Desktop remoto con proxy dell'applicazione di Azure AD](/azure/active-directory/application-proxy-publish-remote-desktop)

Per scoprire in che modo questi servizi semplificano l'architettura della distribuzione di Desktop remoto, vedi [Architetture di Servizi Desktop remoto con ruoli PaaS di Azure univoci](desktop-hosting-logical-architecture.md#rds-architectures-with-unique-azure-paas-roles).