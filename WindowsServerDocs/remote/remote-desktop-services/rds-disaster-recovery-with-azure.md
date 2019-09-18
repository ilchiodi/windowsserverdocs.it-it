---
title: Configurare il ripristino di emergenza per Servizi Desktop remoto con la soluzione di ripristino di emergenza di Azure
description: Informazioni su come usare la soluzione di ripristino di emergenza di Azure per il ripristino di emergenza di una distribuzione di Servizi Desktop remoto
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
ms.openlocfilehash: 79e0364bcb9d2ed899568a6699c61b43d84044e5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871012"
---
# <a name="set-up-disaster-recovery-for-rds-using-azure-site-recovery"></a>Configurare il ripristino di emergenza per Servizi Desktop remoto con Azure Site Recovery

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

È possibile usare Azure Site Recovery per creare una soluzione di ripristino di emergenza per la distribuzione di Servizi Desktop remoto. 

[Azure Site Recovery](/azure/site-recovery/site-recovery-overview) è un servizio basato su Azure che fornisce funzionalità di ripristino di emergenza tramite orchestrazione di replica, failover e ripristino delle macchine virtuali. Azure Site Recovery supporta una serie di tecnologie di replica per eseguire la replica in modo coerente, garantire la protezione ed effettuare il failover uniforme di macchine virtuali e applicazioni in cloud privati, pubblici o ospitati. 

Usa le informazioni seguenti per creare e convalidare la soluzione di ripristino di emergenza.

## <a name="disaster-recovery-deployment-options"></a>Opzioni di distribuzione del ripristino di emergenza

È possibile distribuire Servizi Desktop remoto in server fisici o in macchine virtuali che eseguono Hyper-V o VMWare. Azure Site Recovery è in grado di proteggere le distribuzioni sia locali che virtuali in un sito secondario o in Azure. La tabella seguente illustra le diverse distribuzioni di Servizi Desktop remoto supportate in scenari di ripristino di emergenza da sito a sito e da sito ad Azure.

| Tipo di distribuzione                          | Da sito Hyper-V a sito | Da sito Hyper-V ad Azure | Da sito VMWare ad Azure | Da sito fisico ad Azure |
|------------------------------------------|----------------------|-----------------------|---------------------|----------------------|-----------------------|------------------------|
| Desktop virtuale in pool (non gestito)       |Sì|No|No|No |
| Desktop virtuale in pool (gestito, senza dischi dei profili utente) | Sì|No|No|No|
| RemoteApp e sessioni desktop (senza dischi dei profili utente) | Sì|Sì|Sì|Sì  |

## <a name="prerequisites"></a>Prerequisiti

Prima di configurare Azure Site Recovery per la distribuzione, assicurati che siano soddisfatti i requisiti seguenti:

- Crea una [distribuzione locale di Servizi Desktop remoto](rds-deploy-infrastructure.md).
- Aggiungi un [insieme di credenziali dei servizi Azure Site Recovery](/azure/site-recovery/site-recovery-vmm-to-azure#create-a-recovery-services-vault) alla sottoscrizione di Microsoft Azure.
- Se intendi usare Azure come sito di ripristino, esegui lo [strumento di valutazione dello stato di preparazione delle macchine virtuali di Azure](https://azure.microsoft.com/downloads/vm-readiness-assessment/) nelle tue macchine virtuali per verificare che siano compatibili con le macchine virtuali di Azure e i servizi di Azure Site Recovery.
 
## <a name="implementation-checklist"></a>Elenco di controllo per l'implementazione

Esamineremo i diversi passaggi per abilitare i servizi di Azure Site Recovery per la distribuzione di Servizi Desktop remoto in modo più dettagliato, ma di seguito sono riportati i passaggi generali per l'implementazione.

| **Passaggio 1 - Configurare le macchine virtuali per il ripristino di emergenza**                                                                                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hyper-V - Scarica il provider di Microsoft Azure Site Recovery. Installalo nel server VMM o nell'host Hyper-V. Per altre informazioni, vedi [Prerequisiti per la replica in Azure con Azure Site Recovery](/azure/site-recovery/site-recovery-prereq).                                                                                                                             |
| VMWare - Configura il server di protezione, il server di configurazione e i server di destinazione master                                                                                                                                                      |
| **Passaggio 2 - Preparare le risorse**                                                                                                                                                                                                           |
| Aggiungi un [account di archiviazione di Azure](/azure/storage/storage-create-storage-account).                                                                                                                                                                                                              |
| Hyper-V - Scarica l'agente di Servizi di ripristino di Microsoft Azure e installalo nei server host Hyper-V.                                                                                                                                     |
| VMWare - Verifica che il servizio di mobilità sia installato in tutte le macchine virtuali.                                                                                                                                                                           |
| [Abilita la protezione per le macchine virtuali nel cloud VMM, nei siti Hyper-V o nei siti VMWare](rds-enable-dr-with-asr.md).                                                                                                                                                                    |
| **Passaggio 3 - Progettare il piano di ripristino**                                                                                                                                                                                                        |
| Esegui il mapping delle risorse - Esegui il mapping delle reti locali alle reti virtuali di Azure.                                                                                                                                                                              |
| [Crea il piano di ripristino](rds-disaster-recovery-plan.md). |
| Testa il piano di ripristino creando un failover di test. Verifica che tutte le macchine virtuali possano accedere alle risorse necessarie, come Active Directory. Verifica che i reindirizzamenti di rete siano configurati e funzionanti per Servizi Desktop remoto. Per i passaggi dettagliati per il test del piano di ripristino, vedi [Eseguire un failover di test](/azure/site-recovery/site-recovery-test-failover-to-azure)|
| **Passaggio 4 - Eseguire un'analisi del ripristino di emergenza**                                                                                                                                                                                                     |
| Esegui un'analisi del ripristino di emergenza usando failover pianificati e non pianificati. Verifica che tutte le macchine virtuali abbiano accesso alle risorse necessarie, ad esempio Active Directory. Verifica che tutte le macchine virtuali abbiano accesso alle risorse necessarie, ad esempio Active Directory. Per i passaggi dettagliati relativi ai failover e per informazioni su come eseguire le analisi, vedi [Failover in Site Recovery](/azure/site-recovery/site-recovery-failover).|


