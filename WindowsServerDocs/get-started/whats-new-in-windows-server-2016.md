---
title: Novità di Windows Server 2016
description: Quali sono le nuove funzionalità di calcolo, identità, gestione, automazione, rete, sicurezza e archiviazione.
ms.prod: windows-server
ms.date: 05/21/2019
ms.technology: server-general
ms.topic: article
ms.assetid: 2827f332-44d4-4785-8b13-98429087dcc7
author: jasongerend
ms.author: jgerend
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: afcda1d3f94c5f6fa7524317ac21c5540c07895c
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948268"
---
# <a name="whats-new-in-windows-server-2016"></a>Novità di Windows Server 2016

>Si applica a: Windows Server 2016

![Icona che rappresenta un giornale](media/whats-new.png) Per informazioni sulle funzionalità più recenti di Windows, vedi [Novità di Windows Server](whats-new-in-windows-server.md). Questa sezione illustra le funzionalità nuove e modificate di Windows Server&reg; 2016. Le nuove funzionalità e modifiche elencate di seguito sono quelle che più probabilmente avranno il massimo impatto sull'uso di questa versione.

## <a name="computevirtualizationvirtualizationmd"></a>[Compute](../virtualization/virtualization.md)

L'area Virtualizzazione include funzionalità e prodotti di virtualizzazione che permettono a professionisti IT di progettare, distribuire e gestire Windows Server.  

### <a name="general"></a>Generale  
Le macchine fisiche e virtuali sfruttano una maggiore precisione nella sincronizzazione grazie ai miglioramenti introdotti nei servizi Sincronizzazione ora di Win32 e Hyper-V. Windows Server può ora ospitare servizi conformi alle norme future che richiedono un'accuratezza di 1 ms per quanto riguarda l'ora UTC.  

### <a name="hyper-v"></a>Hyper-V  
-   [Novità di Hyper-V in Windows Server 2016](../virtualization/hyper-v/What-s-new-in-Hyper-V-on-Windows.md). Questo argomento illustra le funzionalità nuove e modificate del ruolo Hyper-V in Windows Server 2016, di Hyper-V client in esecuzione su Windows 10 e di Microsoft Hyper-V Server 2016.  

-   [Contenitori di Windows](https://msdn.microsoft.com/virtualization/windowscontainers/containers_welcome):  il supporto per i contenitori di Windows Server 2016 garantisce migliori prestazioni, semplifica la gestione della rete e facilita l'uso dei contenitori di Windows in Windows 10. Per altre informazioni sui contenitori, vedi [Contenitori: Docker, Windows e tendenze](https://azure.microsoft.com/blog/2015/08/17/containers-docker-windows-and-trends/).  

### <a name="nano-server"></a>Nano Server  
Novità di [Nano Server](getting-started-with-nano-server.md). Nano Server include ora un modulo aggiornato per la creazione di immagini di Nano Server, in grado di offrire una maggiore separazione dell'host fisico e delle macchine virtuali guest, oltre al supporto per le diverse edizioni di Windows Server.   

Sono stati apportati inoltre miglioramenti alla Console di ripristino di emergenza, tra cui la separazione delle regole del firewall in entrata e in uscita, nonché la possibilità di ripristino della configurazione di WinRM.  

### <a name="shielded-virtual-machines"></a>Macchine virtuali schermate  
Windows Server 2016 offre una nuova macchina virtuale schermata basata su Hyper-V per proteggere le macchine virtuali di seconda generazione da un'infrastruttura compromessa. Tra le funzionalità introdotte in Windows Server 2016 sono incluse le seguenti:  

- Nuova modalità "Crittografia supportata" che offre protezioni aggiuntive rispetto a una normale macchina virtuale, ma meno rispetto alla modalità "Schermata", supportando comunque vTPM, la crittografia del disco, la crittografia del traffico di Live Migration e altre funzionalità, tra cui i vantaggi dell'amministrazione diretta dell'infrastruttura, ad esempio le connessioni alla console della macchina virtuale e Powershell Direct.  

- Supporto completo per la conversione di macchine virtuali di seconda generazione non schermate in macchine virtuali schermate, includa la crittografia automatica del disco.

- Hyper-V Virtual Machine Manager può ora visualizzare le infrastrutture in cui una macchina virtuale schermata può essere eseguita, offrendo così l'opportunità per l'amministratore dell'infrastruttura di aprire la protezione con chiave della macchina virtuale schermata e di visualizzare le infrastrutture in cui l'esecuzione della macchina è autorizzata.  

- È possibile alternare le modalità Attestazione in un servizio Sorveglianza Host in esecuzione. È ora possibile passare in tempo reale dall'attestazione basata su Active Directory meno sicura, ma più semplice all'attestazione basata su TPM, e viceversa.  

- Strumenti di diagnostica end-to-end basati su Windows PowerShell che sono in grado di rilevare errori o problemi di configurazione sia negli host Hyper-V protetti che nel servizio Sorveglianza host.  

- Un ambiente di ripristino che offre un modo per risolvere i problemi in modo sicuro e ripristinare le macchine virtuali schermate all'interno dell'infrastruttura in cui vengono normalmente eseguite, offrendo lo stesso livello di protezione della macchina virtuale schermata stessa.

- Supporto del servizio Sorveglianza host per un'istanza sicura esistente di Active Directory. È possibile indicare al servizio Sorveglianza host di usare una foresta di Active Directory esistente anziché creare una specifica istanza di Active Directory.

Per altre informazioni e istruzioni per l'uso di macchine virtuali schermate, vedere [Shielded VMs and Guarded Fabric Validation Guide for Windows Server 2016 (TPM)](https://aka.ms/shieldedvms) (Guida alle macchine virtuali schermate e alla convalida delle infrastrutture controllate per Windows Server 2016 (TPM)).  

## <a name="identity-and-accessidentityidentity-and-accessmd"></a>[Identità e accesso](../identity/Identity-and-Access.md)  
Le nuove funzionalità di Identità migliorano la capacità delle organizzazioni di proteggere gli ambienti Active Directory e consentono di eseguire la migrazione a distribuzioni solo cloud e ibride, in cui alcune applicazioni e alcuni servizi sono ospitati nel cloud e altri sono ospitati in locale.  

### <a name="active-directory-certificate-services"></a>Servizi certificati Active Directory  
Servizi certificati Active Directory in Windows Server 2016 aumenta il supporto per l'attestazione della chiave TPM. Puoi usare ora il provider di archiviazione chiavi per smart card per l'attestazione della chiave, mentre i dispositivi non appartenenti al dominio possono usare ora la registrazione NDES per ottenere i certificati che possono essere attestati per le chiavi in un TPM.  

### <a name="active-directory-domain-services"></a>Active Directory Domain Services  
Active Directory Domain Services include miglioramenti che consentono alle organizzazioni di proteggere gli ambienti Active Directory e fornire una migliore esperienza nella gestione delle identità per i dispositivi aziendali e personali. Per altre informazioni, vedere [What's new in Active Directory Domain Services (AD DS) in Windows Server 2016](../identity/whats-new-active-directory-domain-services.md) (Novità di Active Directory Domain Services (AD DS) in Windows Server 2016).   

### <a name="active-directory-federation-services"></a>Active Directory Federation Services  
Novità di Active Directory Federation Services. Active Directory Federation Services (AD FS) in Windows Server 2016 include nuove funzionalità che consentono di configurare AD FS per l'autenticazione degli utenti archiviati nelle directory LDAP (Lightweight Directory Access Protocol). Per altre informazioni, vedi [Novità di AD FS per Windows Server 2016](../identity/ad-fs/overview/whats-new-active-directory-federation-services-windows-server.md).  

### <a name="web-application-proxy"></a>Proxy applicazione Web  
L'ultima versione di Proxy applicazione Web è incentrata sulle nuove funzionalità che permettono la pubblicazione e la preautenticazione per più applicazioni e un'esperienza utente migliorata. Consultare l'elenco completo delle nuove funzionalità che include la preautenticazione per applicazioni rich client quali domini di Exchange ActiveSync e con caratteri jolly per la pubblicazione più semplice di applicazioni SharePoint. Per altre informazioni, vedi [Proxy applicazione Web in Windows Server 2016](../remote/remote-access/web-application-proxy/web-application-proxy-windows-server.md).  

##  <a name="administrationadministrationmanage-windows-servermd"></a>[Amministrazione](../administration/manage-windows-server.md)  
L'area Gestione e automazione è incentrata sulle informazioni di riferimento e degli strumenti per i professionisti IT che vogliono eseguire e gestire Windows Server 2016, incluso Windows PowerShell.

Windows PowerShell 5.1 include nuove funzionalità significative, tra cui il supporto per lo sviluppo con le classi e nuove funzionalità di sicurezza che ne estendono e migliorano l'usabilità, oltre a permettere di controllare e gestire gli ambienti basati su Windows in modo più semplice e completo. Per i dettagli, vedi [Nuovi scenari e funzionalità in WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/scenarios-features).

Le nuove funzionalità aggiunte per Windows Server 2016 includono la possibilità di eseguire PowerShell.exe in locale su Nano Server (non più solo in remoto), i nuovi cmdlet per utenti e gruppi locali che sostituiscono l'interfaccia utente grafica, il supporto per il debug di PowerShell e il supporto per la registrazione di protezione, la trascrizione e la tecnologia JEA (Just Enough Administration) in Nano Server.

Ecco alcune delle altre nuove funzionalità di amministrazione:

### <a name="powershell-desired-state-configuration-dsc-in-windows-management-framework-wmf-5"></a>Configurazione dello stato desiderato tramite PowerShell in Windows Management Framework (WMF) 5
Windows Management Framework 5 include gli aggiornamenti per Configurazione dello stato desiderato tramite PowerShell, Gestione remota Windows (WinRM) e Strumentazione gestione Windows (WMI).

Per altre informazioni sull'esecuzione dei test delle funzionalità DSC di Windows Management Framework 5, vedi la serie di post di blog descritti nella pagina relativa alla [convalida delle funzionalità DSC di PowerShell](https://blogs.msdn.microsoft.com/powershell/2015/07/06/validate-features-of-powershell-dsc/). Per il download, vedi [Windows Management Framework 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

### <a name="packagemanagement-unified-package-management-for-software-discovery-installation-and-inventory"></a>Gestione unificata dei pacchetti PackageManagement per l'individuazione, l'installazione e l'inventario di prodotti software
Windows Server 2016 e Windows 10 includono una nuova funzionalità PackageManagement (denominata in precedenza OneGet) che consente ai professionisti IT o DevOps di automatizzare l'individuazione, l'installazione e l'inventario dei prodotti software, localmente o in remoto, indipendentemente dalla tecnologia di installazione e dalla posizione del software. 

Per altre informazioni, vedi [https://github.com/OneGet/oneget/wiki](https://github.com/OneGet/oneget/wiki).

### <a name="powershell-enhancements-to-assist-digital-forensics-and-help-reduce-security-breaches"></a>Miglioramenti di PowerShell a supporto dell'informatica forense e per consentire di ridurre le violazioni della sicurezza
Per aiutare il team responsabile dell'analisi dei sistemi compromessi, sono state aggiunte a PowerShell opzioni di registrazione e di informatica forense e sono state aggiunte funzionalità per ridurre le vulnerabilità degli script, ad esempio API vincolate di PowerShell e API CodeGeneration sicure.

Per altre informazioni, vedi [PowerShell ♥ the Blue Team](https://blogs.msdn.microsoft.com/powershell/2015/06/09/powershell-the-blue-team/).

## <a name="networkingnetworkingnetworkingmd"></a>[Funzionalità di rete](../networking/Networking.md)  
Questa area si focalizza sui prodotti e sulle funzionalità di rete utilizzabili dai professionisti IT per la progettazione, la distribuzione e la manutenzione di Windows Server 2016.  

### <a name="software-defined-networking"></a>rete definita tramite software
È ora possibile eseguire il mirroring e instradare il traffico verso appliance virtuali nuove o esistenti. Insieme a un firewall distribuito e ai gruppi di sicurezza di rete, consente di effettuare la segmentazione in modo dinamico e proteggere i carichi di lavoro in modo analogo ad Azure. In secondo luogo, è possibile distribuire e gestire tutto lo stack di SDN (Software Defined Networking) utilizzando System Center Virtual Machine Manager. Infine, è possibile usare Docker per gestire la rete di contenitori di Windows Server e associare i criteri di SDN non solo alle macchine virtuali ma anche ai contenitori. Per altre informazioni, vedi [Pianificare un'infrastruttura Software Defined Network](../networking/sdn/plan/plan-a-software-defined-network-infrastructure.md).

### <a name="tcp-performance-improvements"></a>Miglioramenti delle prestazioni delle connessioni TCP
Il valore predefinito di ICW (Initial Congestion Window) è stato aumentato da 4 a 10 ed è stato implementato TFO (TCP Fast Open). TFO riduce la quantità di tempo necessaria per stabilire una connessione TCP e il valore incrementato di ICW consente il trasferimento di oggetti di maggiori dimensioni nel burst iniziale. Questa combinazione può ridurre notevolmente il tempo necessario per trasferire un oggetto Internet tra il client e il cloud.

Per migliorare il comportamento di TCP durante il ripristino da una perdita di pacchetti sono state implementate le funzionalità TLP (TCP Tail Loss Probe) e RACK (Recent Acknowledgement). TLP consente di convertire il timeout di ritrasmissione (RTO) in ripristini veloci e RACK riduce il tempo necessario per il ripristino rapido per ritrasmettere un pacchetto perso. 

## <a name="security-and-assurancesecuritysecurity-and-assurancemd"></a>[Sicurezza e controllo](../security/Security-and-Assurance.md)  
Include funzionalità e soluzioni di protezione per il professionista IT che esegue la distribuzione nell'ambiente cloud e del centro dati. Per informazioni generali sulla sicurezza in Windows Server 2016, vedere [Sicurezza e controllo](../security/Security-and-Assurance.md).  

### <a name="just-enough-administration"></a>JEA  
JEA (Just Enough Administration) in Windows Server 2016 è una tecnologia di protezione che consente l'amministrazione delegata per qualsiasi elemento che può essere gestito con Windows PowerShell. Le funzionalità includono il supporto per l'esecuzione in un'identità di rete, la connessione tramite PowerShell Direct, la copia sicura dei file da o verso gli endpoint JEA e la configurazione della console di PowerShell in modo da poterla avviare per impostazione predefinita in un contesto JEA. Per ulteriori dettagli, vedere [JEA on GitHub](https://aka.ms/JEA) (JEA su GitHub).

### <a name="credential-guard"></a>Credential Guard
Credential Guard usa la protezione basata su virtualizzazione per isolare i segreti in modo che solo il software di sistema con privilegi possa accedervi. Vedere [Proteggere le credenziali di dominio derivate con Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard).

###  <a name="remote-credential-guard"></a>Remote Credential Guard
Credential Guard include il supporto per le sessioni RDP in modo che le credenziali utente rimangano sul lato client e non siano esposte sul lato server. Include inoltre la funzionalità Single Sign-On per Desktop remoto. Vedi [Proteggere le credenziali di dominio derivate con Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).   

### <a name="device-guard-code-integrity"></a>Device Guard (integrità del codice)
Device Guard garantisce l'integrità del codice in modalità kernel (KMCI, Kernel Mode Code Integrity) e in modalità utente (UMCI, User Mode Code Integrity) mediante la creazione di criteri che specificano quale codice può essere eseguito sul server. Vedi [Introduzione a Windows Defender Device Guard: sicurezza basata su virtualizzazione e criteri di integrità del codice](https://docs.microsoft.com/windows/device-security/device-guard/introduction-to-device-guard-virtualization-based-security-and-code-integrity-policies).


### <a name="windows-defender"></a>Windows Defender  
[Panoramica di Windows Defender per Windows Server 2016](../security/windows-defender/windows-defender-overview-windows-server.md). Windows Server Antimalware è installato e abilitato per impostazione predefinita in Windows Server 2016, ma l'interfaccia utente per Windows Server Antimalware non è installata. Tuttavia, Windows Server Antimalware aggiorna le definizioni antimalware e protegge il computer senza l'interfaccia utente. Se è necessaria l'interfaccia utente per Windows Server Antimalware, è possibile installarla dopo l'installazione del sistema operativo usando l'aggiunta guidata ruoli e funzionalità.

### <a name="control-flow-guard"></a>Protezione del flusso di controllo
Questa funzionalità di protezione della piattaforma è stata creata per contrastare le vulnerabilità legate al danneggiamento della memoria. Per altre informazioni, vedere [Control Flow Guard](https://msdn.microsoft.com/library/windows/desktop/mt637065(v=vs.85).aspx) (Protezione del flusso di controllo).


## <a name="storagestoragestoragemd"></a>[Archiviazione](../storage/storage.md)

In Windows Server 2016 la tecnologia di archiviazione include nuove funzionalità e miglioramenti per l'archiviazione software-defined e anche per i file server tradizionali. Di seguito sono riportate alcune delle nuove funzionalità. Per informazioni su altri miglioramenti e per altri dettagli, vedere [Novità di Archiviazione in Windows Server 2016](../storage/whats-new-in-storage.md).

### <a name="storage-spaces-direct"></a>Spazi di archiviazione diretta

Spazi di archiviazione diretta consente di creare un'archiviazione altamente disponibile e scalabile con server di archiviazione locale. Questa funzionalità semplifica la distribuzione e la gestione dei sistemi con archiviazione definita dal software e consente l'uso di nuove classi di dispositivi disco, ad esempio le unità SSD SATA e NVMe, che in precedenza non erano disponibili con spazi di archiviazione raggruppati in cluster che includevano i dischi condivisi.

Per altre informazioni, vedere [Spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-overview.md).

### <a name="storage-replica"></a>Replica archiviazione

Replica di archiviazione consente di eseguire la replica sincrona a livello di blocco, indipendente dall'archiviazione, tra server o cluster per il ripristino di emergenza, nonché l'estensione di un cluster di failover tra siti. La replica sincrona consente il mirroring dei dati in siti fisici con volumi coerenti per arresto anomalo del sistema senza perdere dati a livello di file system. La replica asincrona consente l'estensione del sito oltre le aree metropolitane con la possibilità di perdita di dati.

Per altre informazioni, vedere [Replica di archiviazione](../storage/storage-replica/storage-replica-overview.md).

### <a name="storage-quality-of-service-qos"></a>QoS di archiviazione

È ora possibile usare QoS di archiviazione per monitorare in modo centralizzato le prestazioni di archiviazione end-to-end e creare criteri di gestione usando cluster Hyper-V e CSV in Windows Server 2016.

Per altre informazioni, vedere [Storage Quality of Service](../storage/storage-qos/storage-qos-overview.md) (QoS di archiviazione).

## <a name="failover-clusteringfailover-clusteringwhats-new-in-failover-clusteringmd"></a>[Clustering di failover](../failover-clustering/whats-new-in-failover-clustering.md)

Windows Server 2016 include una serie di nuove funzionalità e miglioramenti per più server raggruppati in un unico cluster a tolleranza di errore mediante la funzione Clustering di failover. Alcune delle funzionalità aggiunte sono elencate di seguito. Per un elenco completo, vedere [Novità del clustering di failover in Windows Server 2016](../failover-clustering/whats-new-in-failover-clustering.md).

### <a name="cluster-operating-system-rolling-upgrade"></a>Aggiornamento in sequenza del sistema operativo del cluster

L'aggiornamento in sequenza del sistema operativo del cluster consente a un amministratore di aggiornare il sistema operativo dei nodi del cluster da Windows Server 2012 R2 a Windows Server 2016 senza interrompere i carichi di lavoro del file server di scalabilità orizzontale o di Hyper-V. Usando questa funzionalità, è possibile evitare le sanzioni per il tempo di inattività previste dai contratti di servizio.

Per altre informazioni, vedere [Aggiornamento in sequenza del sistema operativo del cluster](../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md).

### <a name="cloud-witness"></a>Cloud di controllo

Cloud di controllo è un nuovo tipo di quorum di controllo per un cluster di failover in Windows Server 2016 che si basa su Microsoft Azure come punto di arbitraggio. Cloud di controllo, come altri quorum di controllo, ottiene un voto e può partecipare ai calcoli del quorum. È possibile configurare questa funzionalità come quorum di controllo usando la Configurazione guidata quorum del cluster.

Per altre informazioni, vedere [Deploy Cloud Witness](../failover-clustering/deploy-cloud-witness.md) (Distribuire un cloud di controllo).

### <a name="health-service"></a>Servizio integrità

Il Servizio integrità migliora le attività giornaliere di monitoraggio, esecuzione e manutenzione delle risorse in un cluster di Spazi di archiviazione diretta.

Per altre informazioni, vedere [Servizio integrità](../failover-clustering/health-service-overview.md).

## <a name="application-development"></a>Sviluppo di applicazioni

### <a name="internet-information-services-iis-100"></a>Internet Information Services (IIS) 10.0
Le nuove funzionalità fornite dal server Web IIS 10.0 in Windows Server 2016 includono:

- Supporto per il protocollo HTTP/2 nello stack di rete e integrato con IIS 10.0. In questo modo i siti Web IIS 10.0 possono gestire automaticamente le richieste HTTP/2 per le configurazioni supportate. Ciò permette numerosi miglioramenti rispetto a HTTP/1.1, ad esempio un riutilizzo delle connessioni più efficiente e una latenza ridotta, migliorando i tempi di caricamento delle pagine Web. 
- Possibilità di eseguire e gestire IIS 10.0 in Nano Server. Vedi [IIS in Nano Server](iis-on-nano-server.md).
- Supporto per le intestazioni host con caratteri jolly, che permette agli amministratori di configurare un server Web per un dominio e quindi impostare il server Web in modo che gestisca le richieste di qualsiasi sottodominio.
- Un nuovo modulo di PowerShell (IISAdministration) per la gestione di IIS. 

Per altri dettagli, vedi [IIS](https://iis.net/learn).

### <a name="distributed-transaction-coordinator-msdtc"></a>Distributed Transaction Coordinator (MSDTC)
Sono state aggiunte tre nuove funzionalità in Microsoft Windows 10 e Windows Server 2016:

- Una nuova interfaccia per la funzione Rejoin di Gestione risorse è utilizzabile da un gestore delle risorse per determinare il risultato di una transazione in dubbio dopo il riavvio di un database a causa di un errore. Vedi [IResourceManagerRejoinable::Rejoin](https://msdn.microsoft.com/library/mt203799(v=vs.85).aspx) per informazioni dettagliate.

- Il limite per i nomi DSN è stato aumentato da 256 byte a 3072 byte. Per i dettagli, vedi [IDtcToXaHelperFactory::Create](https://msdn.microsoft.com/library/ms686861(v=vs.85).aspx), [IDtcToXaHelperSinglePipe::XARMCreate](https://msdn.microsoft.com/library/ms679248(v=vs.85).aspx) o [IDtcToXaMapper::RequestNewResourceManager](https://msdn.microsoft.com/library/ms680310(v=vs.85).aspx).

- La migliorata funzione di traccia ti permette di impostare una chiave del Registro di sistema per includere un percorso di file di immagine nel nome del file di log di traccia in modo da indicare quale file di log di traccia controllare. Vedi [Come abilitare la traccia di diagnostica per MS DTC in un computer basato su Windows](https://support.microsoft.com/kb/926099) per informazioni dettagliate sulla configurazione della traccia per MSDTC.



## <a name="see-also"></a>Vedere anche  
-   [Note sulla versione: problemi importanti di Windows Server 2016](Windows-Server-2016-GA-Release-Notes.md)  

