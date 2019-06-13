---
title: La connessione di Windows Server a servizi di Azure hybrid
description: È possibile estendere le distribuzioni locali di Windows Server nel cloud usando servizi di Azure hybrid.
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 05/31/2019
ms.openlocfilehash: 460399a57bc229b44d37a9fdd1e4938bf9e7d6ac
ms.sourcegitcommit: fe621b72d45d0259bac1d5b9031deed3dcbed29d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2019
ms.locfileid: "66455359"
---
# <a name="connecting-windows-server-to-azure-hybrid-services"></a>La connessione di Windows Server a servizi di Azure hybrid

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale)

È possibile estendere le distribuzioni locali di Windows Server nel cloud usando servizi di Azure hybrid. Questi servizi cloud offrono una matrice di funzioni utili, incluse le seguenti:

- Proteggere le macchine virtuali e usare cloud in base al backup e ripristino di emergenza (disponibilità elevata e ripristino di emergenza) con Azure Site Recovery. 
- Tenere traccia di ciò che avviene tra applicazioni, rete e infrastruttura con l'aiuto di analitica avanzata e machine learning in Monitoraggio di Azure. 
- Semplificare la connettività di rete in Azure con scheda di rete di Azure.
- Mantenere le macchine virtuali aggiornate con gestione aggiornamenti di Azure.

Servizi ibridi di Azure funzionano con Windows Server nelle configurazioni seguenti:

- Server fisico autonomo e macchine virtuali (VM)
- I cluster, inclusi i cluster iperconvergenti certificati dal [Azure Stack uomo](https://docs.microsoft.com/azure-stack/operator/azure-stack-hci-overview), e [Windows Server Software-Defined (WSSD)](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter) programmi

Anche se è possibile impostare più ibrido servizi tramite il portale di Azure e un download o due, molte sono integrate direttamente in Windows Admin Center per fornire un'esperienza di installazione semplificata e una visualizzazione incentrata sul server dei servizi.

## <a name="azure-hybrid-services-tool"></a>Strumento Servizi ibrida di Azure

Strumento Servizi di Azure hybrid [Windows Admin Center](../understand/windows-admin-center.md) consolida tutti i servizi integrati di Azure in un hub centralizzato in cui è possibile individuare facilmente tutti i servizi di Azure disponibili che apportano valore agli on-premises o ambiente ibrido. 

![Schermata di Windows Admin Center che mostra lo strumento Servizi ibridi di Azure](../media/azure-services/ahs-discover.png)

Se ci si connette a un server con servizi di Azure già abilitati, lo strumento Servizi ibrida di Azure funge da un singolo riquadro per visualizzare tutti i servizi abilitati in tale server. È facilmente possibile Introduzione allo strumento pertinente all'interno di Windows Admin Center, avviare nel portale di Azure per la gestione di tali servizi di Azure, o consultare la documentazione più approfondita a tua disposizione. 

![Schermata di Windows Admin Center con servizi di Azure che sono già installati nel server](../media/azure-services/ahs-dayN.png)

Lo strumento di servizi di Azure hybrid, è possibile:
- Eseguire il backup di Windows Server da Windows Admin Center con [Backup di Azure](azure-backup.md)
- Proteggere le macchine virtuali di Hyper-V da Windows Admin Center con [Azure Site Recovery](azure-site-recovery.md)
- Sincronizzazione di file con il cloud, usando [sincronizzazione File di Azure](azure-file-sync.md)
- Gestire gli aggiornamenti del sistema operativo per tutti i server di Windows, sia in locale o nel cloud, con [gestione aggiornamenti di Azure](azure-update-management.md)
- Monitorare i server, sia in locale o nel cloud e configurare gli avvisi con [monitoraggio di Azure](azure-monitor.md)
- Connettere i server in locale a una rete virtuale di Azure con [scheda di rete di Azure](https://aka.ms/WACNetworkAdapter)

## <a name="services-for-stand-alone-servers-and-vms"></a>Servizi per le VM e server autonomi

Questo è l'elenco completo dei servizi di Azure che forniscono la funzionalità in server autonomi e le macchine virtuali:

- **(Nuovo) Sincronizzazione di file con il cloud con [sincronizzazione File di Azure](https://aka.ms/afs)**  
Sincronizza i file in questo server con condivisioni file di Azure. Mantenere tutti i file locali o cloud di usare la suddivisione in livelli e cache solo i più usati i file nel server, la suddivisione in livelli i dati ad accesso sporadico nel cloud. I dati nel cloud possono eseguire il backup, eliminando la necessità di preoccuparsi di backup del server in locale. Inoltre, a più siti-sincronizzazione consentono di mantenere un set di file sincronizzati tra più server.

- **Aggiungere un livello di sicurezza per Windows Admin Center aggiungendo [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) autenticazione**  
È possibile aggiungere un ulteriore livello di sicurezza per Windows Admin Center richiedendo agli utenti di eseguire l'autenticazione usando le identità di Azure Active Directory (Azure AD) per accedere al gateway. Autenticazione di Azure AD consente anche di sfruttare i vantaggi di funzionalità di sicurezza di Azure AD, ad esempio l'accesso condizionale e multi-factor authentication.  
Per altre informazioni, vedere [authentication di configurare Azure AD per Windows Admin Center.](../configure/user-access-control.md#azure-active-directory)  

- **Proteggere le macchine virtuali Hyper-V con [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
È possibile replicare i carichi di lavoro in esecuzione nelle macchine virtuali in modo che l'infrastruttura di business-critical è protetta in caso di emergenza. Windows Admin Center semplifica il programma di installazione e il processo di replica delle macchine virtuali nel server Hyper-V o cluster, rendendo più semplice migliorare la resilienza dell'ambiente con servizio di ripristino di emergenza di Azure Site Recovery.  
Per altre informazioni, vedi [proteggere le macchine virtuali con Azure Site Recovery e Windows Admin Center](azure-site-recovery.md).

- **Eseguire il backup dei server Windows con [Backup di Azure](https://docs.microsoft.com/azure/backup/backup-overview)**  
È possibile eseguire il backup dei server Windows in Azure, aiuta a proteggersi da eliminazioni accidentali o intenzionali ransomware e danneggiamento.  
Per altre informazioni, vedi [eseguire il backup dei server con Backup di Azure](azure-backup.md).

- **Monitorare e ottenere avvisi tramite posta elettronica per tutti i server nell'ambiente in uso con [monitoraggio di Azure per le macchine virtuali](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**  
È possibile usare monitoraggio di Azure, noto anche come macchine virtuali di Insights, per monitorare lo stato del server e gli eventi, creare avvisi di posta elettronica, ottenere una visualizzazione consolidata delle prestazioni del server nell'ambiente e visualizzare le app, sistemi e servizi connessi a un determinato Server.  
Per altre informazioni, vedi [connettere i server di monitoraggio di Azure e configurare notifiche tramite posta elettronica](azure-monitor.md).

- **Gestire centralmente gli aggiornamenti del sistema operativo per tutti i server Windows con [gestione aggiornamenti di Azure](https://docs.microsoft.com/azure/automation/automation-update-management)**  
È possibile gestire gli aggiornamenti e patch per più server e le macchine virtuali da un'unica posizione, anziché su una base per server. Con gestione aggiornamenti di Azure, si può rapidamente valutare lo stato degli aggiornamenti disponibili, pianificare l'installazione degli aggiornamenti necessari e rivedere i risultati della distribuzione per verificare gli aggiornamenti applicabili correttamente. Ciò è possibile che i server sono ospitati da altri provider di cloud, macchine virtuali di Azure o in locale.  
Per altre informazioni, vedi [configurare i server di gestione aggiornamenti di Azure](azure-update-management.md).

- **Connettere i server in locale a una rete virtuale di Azure con un [scheda di rete di Azure](https://aka.ms/WACNetworkAdapter)**  
È possibile aggiungere una scheda di rete di Azure ai tuoi server locali che consentono di connettere in modo sicuro il server a una rete virtuale di Azure.  
Per altre informazioni, vedi [configurare una connessione VPN da punto a sito tra rete virtuale di Azure e un Server Windows locali](https://aka.ms/WACNetworkAdapter).

- **Gestire le macchine virtuali IaaS di Azure con [Windows Admin Center](manage-azure-vms.md)**  
È possibile utilizzare Windows Admin Center per gestire le macchine virtuali di Azure, nonché macchine virtuali locali. Configurare il gateway di Windows Admin Center per la connessione da rete virtuale di Azure, è possibile gestire le macchine virtuali in Azure usando gli strumenti coerenti e semplificati Windows Admin Center disponibili. Per altre informazioni, vedi [configura Windows Admin Center per gestire le macchine virtuali in Azure](manage-azure-vms.md).

## <a name="services-for-clusters"></a>Servizi per i cluster

Questi sono i servizi di Azure che forniscono funzionalità per i cluster nel suo complesso (questi non sono tutti integrati in Windows Admin Center ancora):

- [Monitorare un cluster iperconvergente con monitoraggio di Azure](../../../storage/storage-spaces/configure-azure-monitor.md)
- [Proteggere le macchine virtuali con Azure Site Recovery](azure-site-recovery.md)
- [Distribuire un cloud di controllo di cluster](../../../failover-clustering/deploy-cloud-witness.md)

## <a name="see-also"></a>Vedi anche

- [Connettere Windows Admin Center ad Azure](azure-integration.md)
- [Distribuire Windows Admin Center in Azure](deploy-wac-in-azure.md)