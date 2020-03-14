---
title: Connessione di Windows Server a servizi ibridi di Azure
description: Per estendere le distribuzioni locali di Windows Server al cloud, puoi usare i servizi ibridi di Azure.
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 05/31/2019
ms.openlocfilehash: b82d2eaa9283d99993102f1656262e2eda86cfff
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323323"
---
# <a name="connecting-windows-server-to-azure-hybrid-services"></a>Connessione di Windows Server a servizi ibridi di Azure

Per estendere le distribuzioni locali di Windows Server al cloud, puoi usare i servizi ibridi di Azure. Questi servizi cloud offrono una serie di funzioni utili che consentono di estendere le funzionalità locali in Azure e di eseguire la gestione centralizzata da Azure.

![Diagramma che mostra una freccia dai server locali al cloud per l'estensione delle funzionalità locali in Azure e una freccia dal cloud ai server locali per la gestione centralizzata con Azure](../media/azure-services/hybrid-framing.png)

Usando i servizi ibridi di Azure in Windows Admin Center, puoi:

- [Proteggere le macchine virtuali e usare funzionalità di backup e ripristino di emergenza basate sul cloud (disponibilità elevata/ripristino di emergenza)](#back-up-and-protect-your-on-premises-servers-and-vms).  
- [Estendere la capacità locale con risorse di archiviazione e calcolo in Azure e semplificare la connettività di rete ad Azure](#extend-on-premises-capacity-with-azure).
- [Centralizzare il monitoraggio, la governance, la configurazione e la sicurezza tra applicazioni, rete e infrastruttura con l'ausilio di servizi di gestione di Azure intelligenti per il cloud](#centrally-manage-your-hybrid-environment-from-azure).  

Anche se puoi installare la maggior parte dei servizi ibridi tramite il portale di Azure scaricando un'app ed eseguendo alcune configurazioni manuali, molti di questi servizi sono integrati direttamente in Windows Admin Center per offrire un'esperienza di installazione semplificata e una visualizzazione dei servizi incentrata sul server. Windows Admin Centers fornisce anche utili collegamenti ipertestuali intelligenti ai portale di Azure per visualizzare le risorse di Azure connesse e offrire una visualizzazione centralizzata dell'ambiente ibrido.

## <a name="discover-integrated-services-in-the-azure-hybrid-services-tool"></a>Individuare servizi integrati nello strumento dei servizi ibridi di Azure

Lo strumento dei servizi ibridi di Azure incluso in [Windows Admin Center](../overview.md) riunisce tutti i servizi integrati di Azure in un hub centralizzato in cui è possibile individuare facilmente tutti i servizi di Azure disponibili utili per l'ambiente locale o ibrido.  

![Screenshot di Windows Admin Center con lo strumento dei servizi ibridi di Azure](../media/azure-services/ahs-discover.png)

Se ti connetti a un server in cui i servizi di Azure sono già abilitati, lo strumento dei servizi ibridi di Azure ti consentirà di visualizzare tutti i servizi abilitati in tale server. Puoi accedere facilmente allo strumento che ti interessa dall'interno di Windows Admin Center, avviare il portale di Azure per una gestione più approfondita dei servizi di Azure oppure consultare la documentazione sempre a tua disposizione.  

![Screenshot di Windows Admin Center con i servizi di Azure già installati nel server](../media/azure-services/ahs-dayN.png)

Con lo strumento dei servizi ibridi di Azure puoi:

- Eseguire il backup del server Windows da Windows Admin Center con [Backup di Azure](azure-backup.md)
- Proteggere le macchine virtuali Hyper-V da Windows Admin Center con [Azure Site Recovery](azure-site-recovery.md)
- Sincronizzare il file server con il cloud con [Sincronizzazione file di Azure](azure-file-sync.md)
- Gestire gli aggiornamenti del sistema operativo per tutti i server Windows, sia in locale che nel cloud, con [Gestione aggiornamenti di Azure](azure-update-management.md)
- Monitorare i server, sia in locale che nel cloud, nonché configurare gli avvisi con [Monitoraggio di Azure](azure-monitor.md)
- Applicare criteri di governance ai server locali tramite Criteri di Azure usando [Azure Arc per server](https://docs.microsoft.com/azure/azure-arc/servers/overview)
- Proteggere i server e ottenere una protezione avanzata dalle minacce con il [Centro sicurezza di Azure](https://docs.microsoft.com/azure/security-center/windows-admin-center-integration)
- Connettere i server locali a una rete virtuale di Azure con la [scheda di rete di Azure](https://aka.ms/WACNetworkAdapter)
- Rendere le macchine virtuali di Azure simili alla rete locale con la [rete estesa di Azure](https://go.microsoft.com/fwlink/?linkid=2109517&clcid=0x409)

## <a name="back-up-and-protect-your-on-premises-servers-and-vms"></a>Eseguire il backup e la protezione dei server e delle macchine virtuali locali

- **Backup dei server Windows con [Backup di Azure](https://docs.microsoft.com/azure/backup/backup-overview)**  
Puoi eseguire il backup dei server Windows in Azure per proteggerti da eliminazioni accidentali o intenzionali, danneggiamento dei dati e ransomware.  
Per altre informazioni, vedi [Eseguire il backup dei server con Backup di Azure](azure-backup.md).

- **Protezione delle macchine virtuali Hyper-V con [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
Puoi replicare i carichi di lavoro in esecuzione nelle macchine virtuali in modo che l'infrastruttura critica dell'azienda sia protetta in caso di emergenza. Windows Admin Center semplifica la configurazione e il processo di replica delle macchine virtuali nei server o nei cluster Hyper-V, rafforzando più facilmente la resilienza dell'ambiente grazie al servizio di ripristino di emergenza di Azure Site Recovery.  
Per altre informazioni, vedi [Proteggere le macchine virtuali con Azure Site Recovery e Windows Admin Center](azure-site-recovery.md).

- **Uso della replica sincrona o asincrona basata su blocchi in una macchina virtuale in Azure tramite la [replica di archiviazione](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-overview)**  
Puoi configurare la replica basata su blocchi o basata su volume da server a server usando la replica di archiviazione in un server secondario o in una macchina virtuale. Windows Admin Center consente di creare una macchina virtuale di Azure in modo specifico per la destinazione della replica, permettendo di dimensionare e configurare correttamente l'archiviazione in una nuova macchina virtuale di Azure.  
Per altre informazioni, vedi [Replica da server a server con la replica di archiviazione](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui).  

## <a name="extend-on-premises-capacity-with-azure"></a>Estendere la capacità locale con Azure

### <a name="extend-storage-capacity"></a>Estendere la capacità di archiviazione

- **Sincronizzazione del file server con il cloud tramite [Sincronizzazione file di Azure](https://aka.ms/afs)**  
Sincronizza i file in questo server con le condivisioni file di Azure. Mantieni tutti i file in locale oppure usa il cloud a livelli per liberare spazio e memorizzare nella cache del server solo i file più usati, spostando nel cloud i dati ad accesso sporadico. È possibile eseguire il backup dei dati nel cloud per non preoccuparsi più del backup del server in locale. La sincronizzazione multisito consente di mantenere un set di file sincronizzato in più server.
Per altre informazioni, vedi [Sincronizzare il file server con il cloud con Sincronizzazione file di Azure](azure-file-sync.md).

- **Migrazione dell'archiviazione in una macchina virtuale di Azure tramite il [servizio di migrazione della risorsa di archiviazione](https://docs.microsoft.com/windows-server/storage/storage-migration-service/overview)**  
Usa lo strumento con istruzioni dettagliate per eseguire l'inventario dei dati nei server Windows e Linux e quindi trasferire i dati in una nuova macchina virtuale di Azure. Windows Admin Center può creare una nuova macchina virtuale di Azure per il processo con dimensioni appropriate e configurato correttamente per la ricezione dei dati dal server di origine.  
Per altre informazioni, vedi [Usare il servizio di migrazione della risorsa di archiviazione per eseguire la migrazione di un server](https://docs.microsoft.com/windows-server/storage/storage-migration-service/migrate-data).

### <a name="extend-compute-capacity"></a>Estendere la capacità di calcolo

- **Creazione di una nuova macchina virtuale di Azure senza uscire da Windows Admin Center**  
Dalla pagina *Tutte le connessioni* in Windows Admin Center passare ad **Aggiungi** e selezionare **Crea nuovo** in **Azure VM** (Macchina virtuale di Azure). Puoi anche aggiungere la macchina virtuale di Azure al dominio e configurare l'archiviazione in questo strumento per la creazione passo passo.

- **Uso di Azure per ottenere il quorum nel cluster di failover con [Cloud di controllo](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)**  
Anziché investire in hardware aggiuntivo per ottenere il quorum in un cluster a due nodi, puoi usare un account di archiviazione di Azure che funga da cluster di controllo per il cluster Azure Stack HCI o un altro cluster di failover.  
Per altre informazioni, vedere [Deploy a Cloud Witness for a Failover Cluster](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness) (Distribuire un cloud di controllo per un cluster di failover).  

### <a name="simplify-network-connectivity-between-your-on-premises-and-azure-networks"></a>Semplificare la connettività di rete tra le reti locali e di Azure

- **Connessione dei server locali a una rete virtuale di Azure con la [scheda di rete di Azure](https://aka.ms/WACNetworkAdapter)**  
Windows Admin Center può semplificare la configurazione di una VPN da punto a sito da un server locale a una rete virtuale di Azure.  

- **Configurazione delle macchine virtuali di Azure in modo che siano simili alla rete locale con la [rete estesa di Azure](https://go.microsoft.com/fwlink/?linkid=2109517&clcid=0x409)**  
Windows Admin Center può configurare una VPN da sito a sito ed estendere gli indirizzi IP locali nella rete virtuale di Azure per consentire di eseguire più facilmente la migrazione dei carichi di lavoro in Azure senza suddividere le dipendenze su indirizzi IP.

## <a name="centrally-manage-your-hybrid-environment-from-azure"></a>Eseguire la gestione centralizzata di un ambiente ibrido da Azure

- **Monitoraggio e ricezione di avvisi di posta elettronica per tutti i server presenti nell'ambiente con [Monitoraggio di Azure per le macchine virtuali](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**  
Puoi usare Monitoraggio di Azure, noto anche come Informazioni dettagliate sulle macchine virtuali, per monitorare eventi e integrità del server, creare avvisi di posta elettronica, ottenere una visualizzazione consolidata delle prestazioni del server nell'ambiente, nonché visualizzare app, sistemi e servizi connessi a un determinato server. Windows Admin Center può inoltre impostare avvisi di posta elettronica predefiniti per eventi relativi allo stato delle prestazioni del server e allo stato del cluster.  
Per altre informazioni, vedi [Connettere i server a Monitoraggio di Azure e configurare le notifiche di posta elettronica](azure-monitor.md).

- **Gestione centralizzata degli aggiornamenti del sistema operativo per tutti i server Windows con [Gestione aggiornamenti di Azure](https://docs.microsoft.com/azure/automation/automation-update-management)**  
Puoi gestire aggiornamenti e patch per più server e macchine virtuali da un'unica posizione, anziché per singolo server. Con Gestione aggiornamenti di Azure puoi valutare rapidamente lo stato degli aggiornamenti disponibili, pianificare l'installazione degli aggiornamenti obbligatori ed esaminare i risultati della distribuzione per verificare che gli aggiornamenti siano stati applicati correttamente. Queste operazioni sono possibili sia che i server siano locali oppure che siano macchine virtuali di Azure, ospitati da altri provider di servizi cloud.  
Per altre informazioni, vedi [Configurare i server per Gestione aggiornamenti di Azure](azure-update-management.md).

- **Ottimizzazione del comportamento di sicurezza e possibilità di ottenere una protezione avanzata dalle minacce con il [Centro sicurezza di Azure](https://docs.microsoft.com/azure/security-center/security-center-intro)**  
Il Centro sicurezza di Azure è un sistema di gestione della sicurezza dell'infrastruttura unificato che potenzia il comportamento di sicurezza dei data center e offre una protezione avanzata dalle minacce nei carichi di lavoro ibridi in locale o nel cloud, indipendentemente dal fatto che si trovino in Azure o meno. Con Windows Admin Center puoi configurare e connettere facilmente i server al Centro sicurezza di Azure.  
Per altre informazioni, vedi [Integrare il Centro sicurezza di Azure con Windows Admin Center (anteprima)](https://docs.microsoft.com/azure/security-center/windows-admin-center-integration).  

- **Applicazione dei criteri e garanzia della conformità in un ambiente ibrido con [Azure Arc per server](https://docs.microsoft.com/azure/azure-arc/servers/overview) e [Criteri di Azure](https://docs.microsoft.com/azure/governance/policy/overview)**  
Esegui l'inventario, organizza e gestisci i server locali da Azure. Puoi amministrare i server usando i criteri di Azure, controllare l'accesso con Controllo degli accessi in base al ruolo e abilitare servizi di gestione aggiuntivi da Azure.  

## <a name="clusters-versus-stand-alone-servers-and-vms"></a>Confronto tra cluster e macchine virtuali e server autonomi

È possibile usare i servizi ibridi di Azure con i server Windows nelle configurazioni seguenti:

- Server fisici e macchine virtuali autonome
- Cluster, inclusi i cluster iperconvergenti certificati dai programmi [Azure Stack HCI](../../../azure-stack-hci/index.md) e [software-defined per Windows Server](https://www.microsoft.com/cloud-platform/software-defined-datacenter)

### <a name="services-for-stand-alone-servers-and-vms"></a>Servizi per server e macchine virtuali autonome

Ecco l'elenco completo dei servizi di Azure utilizzabili con server e macchine virtuali autonome:

- Eseguire il backup del server Windows da Windows Admin Center con [Backup di Azure](azure-backup.md)
- Proteggere le macchine virtuali Hyper-V da Windows Admin Center con [Azure Site Recovery](azure-site-recovery.md)
- Sincronizzare il file server con il cloud con [Sincronizzazione file di Azure](azure-file-sync.md)
- Gestire gli aggiornamenti del sistema operativo per tutti i server Windows, sia in locale che nel cloud, con [Gestione aggiornamenti di Azure](azure-update-management.md)
- Monitorare i server, sia in locale che nel cloud, nonché configurare gli avvisi con [Monitoraggio di Azure](azure-monitor.md)
- Applicare criteri di governance ai server locali tramite Criteri di Azure usando [Azure Arc per server](https://docs.microsoft.com/azure/azure-arc/servers/overview)
- Proteggere i server e ottenere una protezione avanzata dalle minacce con il [Centro sicurezza di Azure](https://docs.microsoft.com/azure/security-center/windows-admin-center-integration)
- Connettere i server locali a una rete virtuale di Azure con la [scheda di rete di Azure](https://aka.ms/WACNetworkAdapter)
- Rendere le macchine virtuali di Azure simili alla rete locale con la [rete estesa di Azure](https://go.microsoft.com/fwlink/?linkid=2109517&clcid=0x409)

### <a name="services-for-clusters"></a>Servizi per i cluster

Questi sono i servizi di Azure che forniscono funzionalità ai cluster nel loro complesso:

- [Monitorare un cluster iperconvergente con Monitoraggio di Azure](../../../storage/storage-spaces/configure-azure-monitor.md)
- [Proteggere le macchine virtuali con Azure Site Recovery](azure-site-recovery.md)
- [Distribuire un cloud di controllo cluster](../../../failover-clustering/deploy-cloud-witness.md)  

## <a name="other-azure-integrated-abilities-of-windows-admin-center"></a>Altre funzionalità di Windows Admin Center integrate in Azure

- **[Aggiunta di connessioni a macchine virtuali di Azure](manage-azure-vms.md) in Windows Admin Center**  
Puoi usare Windows Admin Center per gestire sia macchine virtuali di Azure che computer locali. Configurando il gateway di Windows Admin Center per la connessione alla rete virtuale di Azure, puoi gestire macchine virtuali in Azure usando gli strumenti semplificati e coerenti forniti a Windows Admin Center.  
Per altre informazioni, vedi [Configurare Windows Admin Center per la gestione delle macchine virtuali in Azure](manage-azure-vms.md).

- **Aggiunta di un livello di sicurezza a Windows Admin Center con l'autenticazione [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/)**  
Per aggiungere un ulteriore livello di sicurezza a Windows Admin Center, puoi richiedere agli utenti di eseguire l'autenticazione usando identità di Azure Active Directory (Azure AD) per accedere al gateway. L'autenticazione Azure AD consente anche di sfruttare le funzionalità di sicurezza di Azure AD, come l'accesso condizionale e l'autenticazione a più fattori.  
Per altre informazioni, vedi [Configurare l'autenticazione Azure AD per Windows Admin Center](../configure/user-access-control.md#azure-active-directory).  

- **Gestione delle risorse di Azure direttamente tramite il servizio [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) incorporato in Windows Admin Center**  
Puoi sfruttare Azure Cloud Shell per ottenere un'esperienza Bash o PowerShell in Windows Admin Center per accedere facilmente alle attività di gestione di Azure.  
Per altre informazioni, vedi [Panoramica di Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).


## <a name="see-also"></a>Vedere anche

- [Connettere Windows Admin Center ad Azure](azure-integration.md)
- [Distribuire Windows Admin Center in Azure](deploy-wac-in-azure.md)
