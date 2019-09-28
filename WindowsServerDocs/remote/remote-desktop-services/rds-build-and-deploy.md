---
title: RDS - Compilazione e distribuzione
description: Procedura per compilare una distribuzione di Desktop remoto
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9c3962a830d9544e915a96f061e32bb9e037949b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404035"
---
# <a name="build-and-deploy-your-remote-desktop-services-deployment"></a>Compilare e distribuire i Servizi Desktop remoto

Una distribuzione di Servizi Desktop remoto è l'infrastruttura utilizzata per condividere app e risorse con gli utenti. A seconda dell'esperienza che si vuole fornire è possibile renderla piccola o complessa, in base alle esigenze. Le distribuzioni di Desktop remoto vengono facilmente ridimensionate. È possibile aumentare e ridurre Accesso Web, Gateway, Gestore connessione e server Host sessione Desktop remoto a discrezione. È possibile usare Gestore Connessione Desktop remoto per distribuire i carichi di lavoro. L'autenticazione basata su Active Directory offre un ambiente a sicurezza elevata. 

[Client Desktop remoto](clients/remote-desktop-clients.md) abilita l'accesso da qualsiasi computer, tablet o telefono Windows, Apple o Android.

Consultare [Architettura di Servizi Desktop remoto](desktop-hosting-logical-architecture.md) per una descrizione dettagliata dei diversi elementi che interagiscono per creare la distribuzione di Servizi Desktop remoto.

Si dispone una distribuzione di Desktop remoto esistente compilata su una versione precedente di Windows Server? È possibile scoprire le opzioni per il passaggio a Windows Server 2016, per sfruttare le funzionalità nuove e migliori in termini prestazioni e ridimensionamento:

- [Eseguire la migrazione della distribuzione di Servizi Desktop remoto a Windows Server 2016](migrate-rds-role-services.md)
- [Aggiornare le distribuzioni di Servizi Desktop remoto a Windows Server 2016](upgrade-to-rds-2016.md)

Si vuole creare una nuova distribuzione di Desktop remoto? Usare le informazioni seguenti per distribuire Desktop remoto in Windows Server 2016:

- [Distribuire un'infrastruttura di Servizi Desktop remoto](rds-deploy-infrastructure.md)
- [Creare una raccolta di sessioni per contenere le app e le risorse che si vuole condividere](rds-create-collection.md)
- [Concedere licenze per la distribuzione di Servizi Desktop remoto](rds-client-access-license.md)
- Chiedere agli utenti di installare un [client Desktop remoto](clients/remote-desktop-clients.md) affinché possano accedere ad app e risorse. 
- Abilitare la disponibilità elevata aggiungendo Gestori connessione e server Host aggiuntivi:
   - [Scalare una raccolta di Servizi Desktop remoto esistente con una farm host sessione Desktop remoto](rds-scale-rdsh-farm.md)
   - [Aggiungere una disponibilità elevata all'infrastruttura di Gestore connessione Desktop remoto](rds-connection-broker-cluster.md)
   - [Aggiungere una disponibilità elevata al front-end Web Desktop remoto e Gateway Desktop remoto](rds-rdweb-gateway-ha.md)
   - [Distribuire un file system di Spazi di archiviazione diretta a due nodi per l'archiviazione di dischi profili utente](rds-storage-spaces-direct-deployment.md)


Se sei un partner di hosting interessato all'uso di Desktop remoto per fornire app e risorse ai clienti o se sei un cliente alla ricerca di qualcuno che ospiti le tue app, consulta [Partner di hosting di Servizi Desktop remoto](rds-hosting-partners.md) per informazioni sulla valutazione che è possibile eseguire in merito all'uso di Servizi Desktop remoto in Azure come ambiente di hosting, nonché un elenco di partner.
