---
title: Servizi Desktop remoto - compilare e distribuire
description: Passaggi per creare una distribuzione di Desktop remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/18/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 176ae424-96e9-4c78-88f5-da418e76c3d7
author: lizap
manager: dongill
ms.openlocfilehash: 0ab4be6ba326db05e8fc6a9c14e84c8cbb855926
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814752"
---
# <a name="build-and-deploy-your-remote-desktop-services-deployment"></a>Compilare e distribuire la distribuzione di Servizi Desktop remoto

Una distribuzione di Servizi Desktop remoto è l'infrastruttura utilizzata per condividere le App e le risorse con gli utenti. A seconda che si desidera fornire l'esperienza, è possibile renderlo di piccole o complesse in base alle esigenze. Le distribuzioni di Desktop remote vengono ridimensionate in modo semplice. È possibile aumentare e ridurre l'accesso Web Desktop remoto, eseguirà il server Gateway, Broker di connessione e sessione Host. È possibile usare Gestore connessione Desktop remoto per distribuire i carichi di lavoro. L'autenticazione di Active Directory basata fornisce un ambiente a sicurezza elevata. 

[Client Desktop remoto](clients\remote-desktop-clients.md) abilitare l'accesso da qualsiasi Windows, Apple o Android computer, tablet o telefono.

Visualizzare [architettura di Servizi Desktop remoto](desktop-hosting-logical-architecture.md) per una descrizione dettagliata dei diversi elementi che funzionano insieme per creare la distribuzione di Servizi Desktop remoto.

Dispone di una distribuzione di Desktop remoto esistente basata su una versione precedente di Windows Server? Scopri le opzioni per il passaggio a WIndows Server 2016, in cui è possibile sfruttare i vantaggi delle funzionalità nuove e migliori prestazioni e scalabilità:

- [Eseguire la migrazione della distribuzione di servizi desktop remoto a Windows Server 2016](migrate-rds-role-services.md)
- [Aggiornare la distribuzione di servizi desktop remoto a Windows Server 2016](upgrade-to-rds-2016.md)

Se si desidera creare una nuova distribuzione di Desktop remoto? Usare le informazioni seguenti per distribuire Desktop remoto in Windows Server 2016:

- [Distribuire l'infrastruttura di Servizi Desktop remoto](rds-deploy-infrastructure.md)
- [Creare un insieme di sessioni per contenere le App e si desidera condividere le risorse](rds-create-collection.md)
- [Concedere in licenza la distribuzione di servizi desktop remoto](rds-client-access-license.md)
- Chiedere agli utenti di installare una [client Desktop remoto](clients/remote-desktop-clients.md) affinché possano accedere l'App e risorse. 
- Abilitare la disponibilità elevata tramite l'aggiunta di altri gestori connessione e sessione host:
   - [Scalare orizzontalmente una raccolta di servizi desktop remoto esistente con una farm Host sessione Desktop remoto](rds-scale-rdsh-farm.md)
   - [Ottenere una disponibilità elevata per l'infrastruttura di Gestore connessione desktop remoto](rds-connection-broker-cluster.md)
   - [Ottenere una disponibilità elevata in primo piano web di Web desktop remoto e Gateway Desktop remoto](rds-rdweb-gateway-ha.md)
   - [Distribuire un sistema file spazi di archiviazione diretta di due nodi per l'archiviazione UPD](rds-storage-spaces-direct-deployment.md)


Se sei un partner di hosting interessato all'uso di Desktop remoto per fornire App e risorse ai clienti o a un cliente cercando qualcuno ospitare le tue App, consultare [Servizi Desktop remoto che ospita i partner](rds-hosting-partners.md) per informazioni su un valutazione che è possibile eseguire sull'uso di servizi desktop remoto in Azure come un ambiente di hosting, nonché un elenco di partner che viene passato.
