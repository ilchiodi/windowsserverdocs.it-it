---
title: Configurare il ripristino di emergenza per Servizi Desktop remoto con il ripristino di emergenza di Azure
description: Informazioni su come usare il ripristino di emergenza di Azure per il ripristino di emergenza per la distribuzione di servizi desktop remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 06/12/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 561a515e23d12cc3397c40fd885550e735ed4d27
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878172"
---
# <a name="set-up-disaster-recovery-for-rds-using-azure-site-recovery"></a>Configurare il ripristino di emergenza per Servizi Desktop remoto con Azure Site Recovery

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile usare Azure Site Recovery per creare una soluzione di ripristino di emergenza per la distribuzione di Servizi Desktop remoto. 

[Azure Site Recovery](/azure/site-recovery/site-recovery-overview) è un servizio basato su Azure che fornisce funzionalità di ripristino di emergenza orchestrando la replica, failover e ripristino delle macchine virtuali. Azure Site Recovery supporta una serie di tecnologie di replica per la replica, proteggere, in modo coerente e facilmente failover di macchine virtuali e applicazioni di privati/pubblici o cloud di hosting. 

Usare le informazioni seguenti per creare e convalidare la soluzione di ripristino di emergenza.

## <a name="disaster-recovery-deployment-options"></a>Opzioni di distribuzione di ripristino di emergenza

È possibile distribuire servizi desktop remoto in server fisici o macchine virtuali che eseguono Hyper-V o VMWare. Azure Site Recovery può proteggere sia in locale e distribuzioni virtuali in un sito secondario o in Azure. La tabella seguente illustra che le diverse distribuzioni di servizi desktop remoto è supportato in scenari di emergenza site-to-site e site-to-Azure recvoery.

| Tipo di distribuzione                          | Hyper-V site-to-site | Hyper-V site-to-Azure | VMWare site-to-Azure | Fisico sito ad Azure |
|------------------------------------------|----------------------|-----------------------|---------------------|----------------------|-----------------------|------------------------|
| Desktop virtuali in pool (non gestito)       |Yes|No|No|No |
| Desktop virtuali in pool (non gestito, nessun UPD) | Yes|No|No|No|
| Sessioni di RemoteApp e desktop (nessun UPD) | Yes|Yes|Yes|Yes  |

## <a name="prerequisites"></a>Prerequisiti

Prima di poter configurare Azure Site Recovery per la distribuzione, assicurarsi che siano soddisfatti i requisiti seguenti:

- Creare un [distribuzione di servizi desktop remoto locale](rds-deploy-infrastructure.md).
- Aggiungere [insieme di credenziali dei servizi di Azure Site Recovery](/azure/site-recovery/site-recovery-vmm-to-azure#create-a-recovery-services-vault) alla sottoscrizione di Microsoft Azure.
- Se si intende usare Azure come sito di ripristino, eseguire la [dello strumento Azure Virtual Machine Readiness Assessment](https://azure.microsoft.com/downloads/vm-readiness-assessment/) nelle VM per assicurarsi che siano compatibili con macchine virtuali di Azure e servizi di Azure Site Recovery.
 
## <a name="implementation-checklist"></a>Elenco di controllo di implementazione

Illustreremo i vari passaggi per abilitare Servizi di Azure Site Recovery per la distribuzione di servizi desktop remoto in modo più dettagliato, ma Ecco i passaggi di implementazione di alto livello.

| **Passaggio 1: configurare le macchine virtuali per il ripristino di emergenza**                                                                                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hyper-V - scaricare il Provider di Microsoft Azure Site Recovery. Installarlo sul server VMM o host Hyper-V. Visualizzare [prerequisiti per la replica in Azure usando Azure Site Recovery](/azure/site-recovery/site-recovery-prereq) per informazioni.                                                                                                                             |
| VMWare - configurare il server di protezione dati, server di configurazione e server di destinazione master                                                                                                                                                      |
| **Passaggio 2: preparare le risorse**                                                                                                                                                                                                           |
| Aggiungere un [account di archiviazione Azure](/azure/storage/storage-create-storage-account).                                                                                                                                                                                                              |
| Hyper-V - scaricare l'agente di servizi di ripristino di Microsoft Azure e installarlo nei server host Hyper-V.                                                                                                                                     |
| VMWare - assicurarsi che il servizio mobility viene installato in tutte le VM.                                                                                                                                                                           |
| [Abilitare la protezione per le macchine virtuali in cloud VMM, siti di Hyper-V o VMWare](rds-enable-dr-with-asr.md).                                                                                                                                                                    |
| **Passaggio 3: progettare il piano di ripristino.**                                                                                                                                                                                                        |
| Eseguire il mapping delle risorse - mappa nelle reti locali a reti virtuali di Azure.                                                                                                                                                                              |
| [Creare il piano di ripristino](rds-disaster-recovery-plan.md). |
| Testare il piano di ripristino mediante la creazione di un failover di test. Assicurarsi che tutte le macchine virtuali possono accedere alle risorse necessarie, come Active Directory. Assicurarsi che rete sono configurati reindirizzamenti e funzionanti per Servizi Desktop remoto. Per informazioni dettagliate sul test del piano di ripristino, vedere [eseguire un failover di test](/azure/site-recovery/site-recovery-test-failover-to-azure)|
| **Passaggio 4: eseguire un ripristino di emergenza.**                                                                                                                                                                                                     |
| Eseguire un'esercitazione sul ripristino di emergenza tramite i failover pianificati e non pianificati. Assicurarsi che tutte le macchine virtuali abbiano accesso alle risorse necessarie, ad esempio Active Directory. Assicurarsi che tutte le macchine virtuali abbiano accesso alle risorse necessarie, ad esempio Active Directory. Per i passaggi dettagliati per i failover e su come eseguire esercitazioni, vedere [Failover in Site Recovery](/azure/site-recovery/site-recovery-failover).|


