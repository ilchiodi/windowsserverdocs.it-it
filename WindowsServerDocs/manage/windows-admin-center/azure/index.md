---
title: Connessione di Windows Server a servizi di Azure ibrido
description: È possibile estendere le distribuzioni locali di Windows Server nel cloud tramite i servizi Azure ibrido.
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: 625bd4b79d277dfaa81767cd781c2ba1316d637e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297024"
---
# Connessione di Windows Server a servizi di Azure ibrido

>Si applica a: Windows Server 2019, Windows Server 2016

È possibile estendere le distribuzioni locali di Windows Server nel cloud tramite i servizi Azure ibrido. Questi servizi cloud forniscono una matrice di utili funzioni, tra cui le seguenti:

- Proteggere le macchine virtuali e usare basata sul cloud backup e ripristino di emergenza (disponibilità elevata/ripristino di emergenza) con Azure Site Recovery. 
- Tenere traccia di ciò che avviene tra le applicazioni, rete e dell'infrastruttura con l'aiuto dei analitica avanzate e l'apprendimento in Azure Monitor. 
- Semplificare la connettività di rete in Azure con scheda di rete di Azure.
- Mantenere aggiornato con gestione aggiornamenti Azure macchine virtuali.

Servizi di Azure ibrido funzionano con Windows Server nelle configurazioni seguenti:

- Autonomo server fisici e macchine virtuali (VM)
- Cluster, inclusi i cluster iper-convergenti certificati da [Azure Stack HCI](../../../azure-stack-hci/index.md)e programmi [Windows_Server_Software-Defined (WSSD)](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter)

Sebbene sia possibile configurare i servizi di hybrid più Azure tramite il portale di Azure e un download o due, molti sono integrati direttamente in Windows Admin Center per fornire un'esperienza di installazione semplificata e una visualizzazione incentrato sul server dei servizi.

## Strumento di servizi di Azure ibrido

Lo strumento di servizi di Azure hybrid in [Windows Admin Center](../understand/windows-admin-center.md) consolida tutti i servizi Azure integrati in un hub centrale in cui puoi facilmente individuare tutti i disponibili servizi di Azure che portano valore ai locale o ibrido ambiente. 

![Screenshot di Windows Admin Center con lo strumento di servizi di Azure ibrido](../media/azure-services/ahs-discover.png)

Se si connette a un server con servizi di Azure già abilitati, lo strumento di servizi di Azure ibrido funge da un unico riquadro di vetro per visualizzare tutti i servizi abilitati in tale server. Puoi facilmente accedere allo strumento pertinente all'interno di Windows Admin Center, effettuare il portale di Azure per la gestione di più approfondita di tali servizi di Azure, o consultare la documentazione avvio a portata di mano. 

![Screenshot di Windows Admin Center con servizi di Azure che sono già installati nel server](../media/azure-services/ahs-dayN.png)

Dallo strumento di servizi di Azure ibrido, puoi:
- Eseguire il backup di Windows Server da Windows Admin Center con [Azure Backup](azure-backup.md)
- Proteggere le macchine virtuali Hyper-V da Windows Admin Center con [Azure Site Recovery](azure-site-recovery.md)
- Sincronizzare il server di file con il cloud, utilizzando [La sincronizzazione di File di Azure](azure-file-sync.md)
- Gestire gli aggiornamenti del sistema operativo per tutti i server di Windows, sia in locale o nel cloud, con [Gestione aggiornamenti Azure](azure-update-management.md)
- Il monitoraggio dei server, sia in locale o nel cloud e configurare gli avvisi con [Monitor di Azure](azure-monitor.md)
- Connettere i server in locale per una rete virtuale di Azure con [Scheda di rete di Azure](https://aka.ms/WACNetworkAdapter)

## Servizi per server autonomi e le macchine virtuali

Questo è l'elenco completo dei servizi di Azure che forniscono funzionalità ai server autonomi e le macchine virtuali:

- **(Nuovo) Sincronizzare il server di file con il cloud tramite [Sincronizzazione di File di Azure](https://aka.ms/afs)**  
Sincronizzare i file nel server con le condivisioni di file Azure. Mantieni tutti i file locali o usare cloud tiering e cache solo l'uso più frequente i file nel server, tiering dati ad accesso sporadico nel cloud. I dati nel cloud possono essere eseguito il backup, eliminando la necessità di preoccuparti di backup del server in locale. Inoltre, multi-site-sincronizzazione possono mantenere un set di file sincronizzati tra più server.

- **Aggiungere un livello di sicurezza di Windows Admin Center mediante l'aggiunta di autenticazione di [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/)**  
Puoi aggiungere un ulteriore livello di sicurezza di Windows Admin Center da richiedere agli utenti di eseguire l'autenticazione usando le identità di Azure Active Directory (Azure AD) per accedere al gateway. Autenticazione di Azure AD consente inoltre di sfruttare funzionalità di sicurezza di Azure Active Directory come accesso condizionale e l'autenticazione a più fattori.  
Per altre info, vedi [ad Azure AD di configurare l'autenticazione di Windows Admin Center.](../configure/user-access-control.md#azure-active-directory)  

- **Proteggere le macchine virtuali Hyper-V con [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
È possibile replicare i carichi di lavoro in esecuzione su macchine virtuali in modo che l'infrastruttura business-critical sia protetta in caso di emergenza. Windows Admin Center semplifica il programma di installazione e il processo di replica delle macchine virtuali nel server Hyper-V o cluster, facilitando l'uso rafforzare la resilienza dell'ambiente con il servizio di ripristino di emergenza dell'Azure Site Recovery.  
Per altre info, vedi [proteggere le macchine virtuali con Azure Site Recovery e Windows Admin Center](azure-site-recovery.md).

- **Backup dei server di Windows con [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview)**  
È possibile eseguire il backup i server Windows Azure, per proteggerti dal ransomware, danneggiamento ed eliminazioni accidentale o dannose.  
Per altre info, vedi [il backup dei server con Azure Backup](azure-backup.md).

- **Monitorare e ottenere avvisi tramite posta elettronica per tutti i server nell'ambiente in uso con [Monitor di Azure per le macchine virtuali](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**  
È possibile utilizzare Azure Monitor, noto anche come macchine virtuali informazioni approfondite, monitorare l'integrità dei server e gli eventi, creare avvisi tramite posta elettronica, ottenere una panoramica consolidata delle prestazioni del server attraverso l'ambiente e ti consente di visualizzare le app, i sistemi e servizi connessi a un determinato Server.  
Per altre info, vedi [connettere i server Azure monitor e configurare le notifiche di posta elettronica](azure-monitor.md).

- **Gestire gli aggiornamenti del sistema operativo in modo centralizzato per tutti i server di Windows con [Gestione aggiornamenti Azure](https://docs.microsoft.com/azure/automation/automation-update-management)**  
Puoi gestire gli aggiornamenti e le patch per più server e le macchine virtuali da un'unica posizione, anziché in base al server. Con gestione aggiornamenti Azure, puoi rapidamente valutare lo stato della disponibilità di aggiornamenti, pianifica l'installazione degli aggiornamenti necessari ed esaminare i risultati di distribuzione per verificare gli aggiornamenti applicabili correttamente. Questo è possibile se i server sono macchine virtuali di Azure, ospitato da altri provider, cloud o locale.  
Per altre info, vedi [Configura server per la gestione degli aggiornamenti di Azure](azure-update-management.md).

- **Connettere i server in locale per una rete virtuale di Azure con una [Scheda di rete di Azure](https://aka.ms/WACNetworkAdapter)**  
Puoi aggiungere una scheda di rete di Azure per i server in locale per aiutarti a connettersi in modo sicuro al server di rete virtuale di Azure.  
Per altre info, vedi [connessione VPN configura un punto a sito tra Windows Server in locale e rete virtuale di Azure](https://aka.ms/WACNetworkAdapter).

- **Gestire le macchine virtuali IaaS di Azure con [Windows Admin Center](manage-azure-vms.md)**  
È possibile utilizzare Windows Admin Center per gestire le macchine virtuali di Azure, nonché computer locale. Configurando il gateway Windows Admin Center di connettersi alla tua con Azure, è possibile gestire macchine virtuali in Azure tramite gli strumenti coerenti e semplificati che Windows Admin Center fornisce. Per altre info, vedi [Configura Windows Admin Center possa gestire macchine virtuali in Azure](manage-azure-vms.md).

## Servizi per i cluster

Questi sono i servizi Azure che forniscono funzionalità per i cluster nel suo complesso (questi non sono tutti integrati in Windows Admin Center ancora):

- [Monitorare un cluster iperconvergente con Monitor di Azure](../../../storage/storage-spaces/configure-azure-monitor.md)
- [Proteggere le macchine virtuali con Azure Site Recovery](azure-site-recovery.md)
- [Distribuire un cloud di controllo di cluster](../../../failover-clustering/deploy-cloud-witness.md)

## Vedi anche

- [Connettersi a Windows Admin Center ad Azure](azure-integration.md)
- [Distribuire Windows Admin Center in Azure](deploy-wac-in-azure.md)