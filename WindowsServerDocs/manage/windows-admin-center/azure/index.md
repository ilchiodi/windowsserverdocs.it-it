---
title: Connessione di Windows Server a servizi ibridi di Azure
description: Per estendere le distribuzioni locali di Windows Server al cloud, puoi usare i servizi ibridi di Azure.
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
# <a name="connecting-windows-server-to-azure-hybrid-services"></a>Connessione di Windows Server a servizi ibridi di Azure

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (Canale semestrale)

Per estendere le distribuzioni locali di Windows Server al cloud, puoi usare i servizi ibridi di Azure. Questi servizi cloud offrono numerose funzioni utili e consentono, ad esempio, di:

- Proteggere le macchine virtuali e usare backup e ripristino di emergenza basato sul cloud (disponibilità elevata/ripristino di emergenza) con Azure Site Recovery. 
- Tenere traccia degli eventi nelle applicazioni, nella rete e nell'infrastruttura, grazie a funzionalità di analisi avanzate e intelligenza artificiale in Monitoraggio di Azure. 
- Semplificare la connettività di rete in Azure con la scheda di rete di Azure.
- Mantenere aggiornate le macchine virtuali con Gestione aggiornamenti di Azure.

È possibile usare i servizi ibridi di Azure con i server Windows nelle configurazioni seguenti:

- Server fisici e macchine virtuali autonome
- Cluster, inclusi i cluster iperconvergenti certificati dai programmi [Azure Stack HCI](https://docs.microsoft.com/azure-stack/operator/azure-stack-hci-overview) e [software-defined per Windows Server](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter)

Anche se è possibile impostare più servizi ibridi tramite il portale di Azure e un paio di download, molti di tali servizi sono integrati direttamente in Windows Admin Center per offrire un'esperienza di installazione semplificata e una visualizzazione dei servizi incentrata sul server.

## <a name="azure-hybrid-services-tool"></a>Strumento dei servizi ibridi di Azure

Lo strumento dei servizi ibridi di Azure incluso in [Windows Admin Center](../understand/windows-admin-center.md) riunisce tutti i servizi integrati di Azure in un hub centralizzato in cui è possibile individuare facilmente tutti i servizi di Azure disponibili utili per l'ambiente locale o ibrido. 

![Screenshot di Windows Admin Center con lo strumento dei servizi ibridi di Azure](../media/azure-services/ahs-discover.png)

Se ti connetti a un server in cui i servizi di Azure sono già abilitati, lo strumento dei servizi ibridi di Azure ti consentirà di visualizzare tutti i servizi abilitati in tale server. Puoi accedere facilmente allo strumento che ti interessa dall'interno di Windows Admin Center, avviare il portale di Azure per una gestione più approfondita dei servizi di Azure oppure consultare la documentazione sempre a tua disposizione. 

![Screenshot di Windows Admin Center con i servizi di Azure già installati nel server](../media/azure-services/ahs-dayN.png)

Con lo strumento dei servizi ibridi di Azure puoi:
- Eseguire il backup del server Windows da Windows Admin Center con [Backup di Azure](azure-backup.md)
- Proteggere le macchine virtuali Hyper-V da Windows Admin Center con [Azure Site Recovery](azure-site-recovery.md)
- Sincronizzare il file server con il cloud con [Sincronizzazione file di Azure](azure-file-sync.md)
- Gestire gli aggiornamenti del sistema operativo per tutti i server Windows, sia in locale che nel cloud, con [Gestione aggiornamenti di Azure](azure-update-management.md)
- Monitorare i server, sia in locale che nel cloud, nonché configurare gli avvisi con [Monitoraggio di Azure](azure-monitor.md)
- Connettere i server locali a una rete virtuale di Azure con la [scheda di rete di Azure](https://aka.ms/WACNetworkAdapter)

## <a name="services-for-stand-alone-servers-and-vms"></a>Servizi per server e macchine virtuali autonome

Ecco l'elenco completo dei servizi di Azure utilizzabili con server e macchine virtuali autonome:

- **(Novità) Sincronizzazione del file server con il cloud con [Sincronizzazione file di Azure](https://aka.ms/afs)**  
Sincronizza i file in questo server con le condivisioni file di Azure. Mantieni tutti i file in locale oppure usa il cloud a livelli per memorizzare nella cache del server solo i file più usati, spostando i dati ad accesso sporadico nel cloud. È possibile eseguire il backup dei dati nel cloud per non preoccuparsi più del backup del server in locale. La sincronizzazione multisito consente di mantenere un set di file sincronizzato in più server.

- **Aggiunta di un livello di sicurezza a Windows Admin Center con l'autenticazione [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/)**  
Per aggiungere un ulteriore livello di sicurezza a Windows Admin Center, puoi richiedere agli utenti di eseguire l'autenticazione usando identità di Azure Active Directory (Azure AD) per accedere al gateway. L'autenticazione Azure AD consente anche di sfruttare le funzionalità di sicurezza di Azure AD, come l'accesso condizionale e l'autenticazione a più fattori.  
Per altre informazioni, vedi [Configurare l'autenticazione Azure AD per Windows Admin Center](../configure/user-access-control.md#azure-active-directory).  

- **Protezione delle macchine virtuali Hyper-V con [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
Puoi replicare i carichi di lavoro in esecuzione nelle macchine virtuali in modo che l'infrastruttura critica dell'azienda sia protetta in caso di emergenza. Windows Admin Center semplifica la configurazione e il processo di replica delle macchine virtuali nei server o nei cluster Hyper-V, rafforzando più facilmente la resilienza dell'ambiente grazie al servizio di ripristino di emergenza di Azure Site Recovery.  
Per altre informazioni, vedi [Proteggere le macchine virtuali con Azure Site Recovery e Windows Admin Center](azure-site-recovery.md).

- **Backup dei server Windows con [Backup di Azure](https://docs.microsoft.com/azure/backup/backup-overview)**  
Puoi eseguire il backup dei server Windows in Azure per proteggerti da eliminazioni accidentali o intenzionali, danneggiamento dei dati e ransomware.  
Per altre informazioni, vedi [Eseguire il backup dei server con Backup di Azure](azure-backup.md).

- **Monitoraggio e ricezione di avvisi di posta elettronica per tutti i server presenti nell'ambiente con [Monitoraggio di Azure per le macchine virtuali](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**  
Puoi usare Monitoraggio di Azure, noto anche come Informazioni dettagliate sulle macchine virtuali, per monitorare eventi e integrità del server, creare avvisi di posta elettronica, ottenere una visualizzazione consolidata delle prestazioni del server nell'ambiente, nonché visualizzare app, sistemi e servizi connessi a un determinato server.  
Per altre informazioni, vedi [Connettere i server a Monitoraggio di Azure e configurare le notifiche di posta elettronica](azure-monitor.md).

- **Gestione centralizzata degli aggiornamenti del sistema operativo per tutti i server Windows con [Gestione aggiornamenti di Azure](https://docs.microsoft.com/azure/automation/automation-update-management)**  
Puoi gestire aggiornamenti e patch per più server e macchine virtuali da un'unica posizione, anziché per singolo server. Con Gestione aggiornamenti di Azure puoi valutare rapidamente lo stato degli aggiornamenti disponibili, pianificare l'installazione degli aggiornamenti obbligatori ed esaminare i risultati della distribuzione per verificare che gli aggiornamenti siano stati applicati correttamente. Queste operazioni sono possibili sia che i server siano locali oppure che siano macchine virtuali di Azure, ospitati da altri provider di servizi cloud.  
Per altre informazioni, vedi [Configurare i server per Gestione aggiornamenti di Azure](azure-update-management.md).

- **Connessione dei server locali a una rete virtuale di Azure con una [scheda di rete di Azure](https://aka.ms/WACNetworkAdapter)**  
Puoi aggiungere una scheda di rete di Azure ai server locali per connettere il server in modo sicuro a una rete virtuale di Azure.  
Per altre informazioni, vedi [Configurare una connessione VPN a punto a sito tra un server Windows locale e una rete virtuale di Azure](https://aka.ms/WACNetworkAdapter).

- **Gestione delle macchine virtuali IaaS di Azure con [Windows Admin Center](manage-azure-vms.md)**  
Puoi usare Windows Admin Center per gestire sia macchine virtuali di Azure che computer locali. Configurando il gateway di Windows Admin Center per la connessione alla rete virtuale di Azure, puoi gestire macchine virtuali in Azure usando gli strumenti semplificati e coerenti forniti a Windows Admin Center. Per altre informazioni, vedi [Configurare Windows Admin Center per la gestione delle macchine virtuali in Azure](manage-azure-vms.md).

## <a name="services-for-clusters"></a>Servizi per i cluster

Ecco i servizi di Azure utilizzabili con i cluster (non tutti sono ancora integrati in Windows Admin Center):

- [Monitorare un cluster iperconvergente con Monitoraggio di Azure](../../../storage/storage-spaces/configure-azure-monitor.md)
- [Proteggere le macchine virtuali con Azure Site Recovery](azure-site-recovery.md)
- [Distribuire un cloud di controllo cluster](../../../failover-clustering/deploy-cloud-witness.md)

## <a name="see-also"></a>Vedi anche

- [Connettere Windows Admin Center ad Azure](azure-integration.md)
- [Distribuire Windows Admin Center in Azure](deploy-wac-in-azure.md)