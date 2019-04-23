---
title: Proteggere la distribuzione di servizi desktop remoto - ripristino di emergenza
description: Scopri le opzioni di ripristino di emergenza per Servizi Desktop remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9ff6a3b0-ea14-424e-9524-209252e9f1a8
author: lizap
ms.author: elizapo
ms.date: 06/12/2017
ms.openlocfilehash: a6eac3a50999633d15b1b6dc28608f60f6fef6c7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834082"
---
# <a name="configure-disaster-recovery-for-remote-desktop-services"></a>Configurare il ripristino di emergenza per Servizi Desktop remoto

Quando si distribuiscono servizi Desktop remoto nell'ambiente, diventa una parte essenziale dell'infrastruttura, in particolare le App e risorse che si condividono con utenti. Se la distribuzione di servizi desktop remoto si arresta a causa di un qualsiasi elemento da un errore di rete a una calamità naturale, gli utenti non possono accedere a tali App e risorse e l'azienda è un rallentamento. Per evitare questo problema, è possibile configurare una soluzione di ripristino di emergenza che consente di eseguire il failover della distribuzione - se la distribuzione di servizi desktop remoto non è disponibile, per qualunque motivo, è disponibile ad assumere la automaticamente un backup.

Per mantenere la distribuzione di servizi desktop remoto in esecuzione nel caso di un singolo componente o computer, è consigliabile configurare la distribuzione di servizi desktop remoto per la disponibilità elevata. È possibile farlo configurando un [farm host sessione Desktop remoto](rds-scale-rdsh-farm.md) e per garantire il [Broker di connessione sono in cluster per la disponibilità elevata](rds-connection-broker-cluster.md). 

Soluzioni di ripristino di emergenza che è consigliabile qui sono per proteggere la distribuzione di grave emergenza - qualcosa che danneggia l'intera distribuzione di servizi desktop remoto (inclusi ruoli ridondanti configurati per la disponibilità elevata). Se rileva una situazione di emergenza, una soluzione di ripristino di emergenza compilato nella distribuzione verrà consentono per eseguire il failover dell'intera distribuzione e ottenere rapidamente le App e le risorse di backup e in esecuzione per gli utenti.

Usare le informazioni seguenti per distribuire soluzioni di ripristino di emergenza in Servizi Desktop remoto:

- [Sfruttare più data center di Azure per verificare che gli utenti possono accedere alla distribuzione di servizi desktop remoto, anche se un data center di Azure diventa inattivo (ridondanza geografica)](rds-multi-datacenter-deployment.md)
- [Distribuire Azure Site Recovery per fornire un failover per i componenti Servizi Desktop remoto nei failover site-to-site o da sito ad Azure](rds-disaster-recovery-with-azure.md)


