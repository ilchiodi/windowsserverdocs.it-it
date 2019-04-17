---
title: Novità di WindowsServer 2016
description: Quali sono le nuove funzionalità di calcolo, identità, gestione, automazione, rete, sicurezza e archiviazione.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 01/05/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2827f332-44d4-4785-8b13-98429087dcc7
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: b504c3396200502a09467ae97a36f9de613e4820
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339389"
---
# Novità di WindowsServer 2016

>Si applica a: WindowsServer 2016

<img src="media/whats-new.png" style='float:left; padding:.5em;' alt="Icon showing a newspaper">Questa sezione illustra le funzionalità nuove e modificate di WindowsServer&reg; 2016. Le nuove funzionalità e modifiche elencate di seguito sono quelle che più probabilmente avranno il massimo impatto sull'uso di questa versione.  
   
<br>
<br>
<br>
<br>
<br>
## [Calcolo](../virtualization/virtualization.md)  
L'area Virtualizzazione include funzionalità e prodotti di virtualizzazione per i professionisti IT che consentono di progettare, distribuire e gestire WindowsServer.  

### Generale  
Le macchine fisiche e virtuali sfruttano una maggiore precisione nella sincronizzazione grazie ai miglioramenti introdotti nei servizi Sincronizzazione ora di Win32 e Hyper-V. WindowsServer può ora ospitare servizi conformi alle norme future che richiedono un'accuratezza di 1 ms per quanto riguarda l'ora UTC.  

### Hyper-V  
-   [Novità di Hyper-V in WindowsServer 2016](../virtualization/hyper-v/What-s-new-in-Hyper-V-on-Windows.md). Questo argomento illustra le funzionalità nuove e modificate del ruolo Hyper-V in WindowsServer 2016, di Hyper-V client in esecuzione su Windows10 e di Microsoft Hyper-V Server 2016.  

-   [Documentazione dei contenitori di Windows](https://msdn.microsoft.com/virtualization/windowscontainers/containers_welcome). Il supporto per i contenitori di WindowsServer 2016 offre migliori prestazioni, semplifica la gestione della rete e facilita l'uso dei contenitori su Windows10. Per informazioni aggiuntive sui contenitori, vedere [Containers: Docker, Windows and Trends](https://azure.microsoft.com/blog/2015/08/17/containers-docker-windows-and-trends/) (Contenitori: Docker, Windows e tendenze).  

### Nano Server  
Novità di [Nano Server](getting-started-with-nano-server.md). Nano Server include ora un modulo aggiornato per la creazione di immagini di Nano Server, in grado di offrire una maggiore separazione dell'host fisico e delle macchine virtuali guest, oltre al supporto per le diverse edizioni di WindowsServer.   

Sono inoltre presenti miglioramenti alla Console di ripristino di emergenza, tra cui la separazione delle regole del firewall in entrata e in uscita, nonché la possibilità di ripristino della configurazione di WinRM.  

### Macchine virtuali schermate  
WindowsServer 2016 offre una nuova macchina virtuale schermata basata su Hyper-V per proteggere le macchine virtuali di seconda generazione da un'infrastruttura compromessa. Tra le funzionalità introdotte in WindowsServer 2016 sono incluse le seguenti:  

- Nuova modalità "Crittografia supportata" che offre protezioni aggiuntive rispetto a una normale macchina virtuale, ma meno rispetto alla modalità "Schermata", supportando comunque vTPM, la crittografia del disco, la crittografia del traffico di Live Migration e altre funzionalità, tra cui i vantaggi dell'amministrazione diretta dell'infrastruttura, ad esempio le connessioni alla console della macchina virtuale e Powershell Direct.  

- Supporto completo per la conversione di macchine virtuali di seconda generazione non schermate in macchine virtuali schermate, includa la crittografia automatica del disco.

- Hyper-V Virtual Machine Manager può ora visualizzare le infrastrutture in cui una macchina virtuale schermata può essere eseguita, offrendo così l'opportunità per l'amministratore dell'infrastruttura di aprire la protezione con chiave della macchina virtuale schermata e di visualizzare le infrastrutture in cui l'esecuzione della macchina è autorizzata.  

- È possibile alternare le modalità Attestazione in un servizio Sorveglianza Host in esecuzione. È ora possibile passare in tempo reale dall'attestazione basata su Active Directory meno sicura, ma più semplice all'attestazione basata su TPM, e viceversa.  

- Strumenti di diagnostica end-to-end basati su Windows PowerShell che sono in grado di rilevare errori o problemi di configurazione sia negli host Hyper-V protetti che nel servizio Sorveglianza host.  

- Un ambiente di ripristino che offre un modo per risolvere i problemi in modo sicuro e ripristinare le macchine virtuali schermate all'interno dell'infrastruttura in cui vengono normalmente eseguite, offrendo lo stesso livello di protezione della macchina virtuale schermata stessa.

- Supporto del servizio Sorveglianza host per un'istanza sicura esistente di Active Directory. È possibile indicare al servizio Sorveglianza host di usare una foresta di Active Directory esistente anziché creare una specifica istanza di Active Directory.

Per altre informazioni e istruzioni per l'uso di macchine virtuali schermate, vedere [Shielded VMs and Guarded Fabric Validation Guide for WindowsServer 2016 (TPM)](https://aka.ms/shieldedvms) (Guida alle macchine virtuali schermate e alla convalida delle infrastrutture controllate per WindowsServer 2016 (TPM)).  

## [Identità e accesso](../identity/Identity-and-Access.md)  
Le nuove funzionalità di Identità migliorano la capacità delle organizzazioni di proteggere gli ambienti Active Directory e consentono di eseguire la migrazione a distribuzioni solo cloud e ibride, in cui alcune applicazioni e alcuni servizi sono ospitati nel cloud e altri sono ospitati in locale.  

### Servizi certificati Active Directory  
Servizi certificati Active Directory (AD CS) in WindowsServer 2016 aumenta il supporto per l'attestazione della chiave TPM. È ora possibile usare il provider di archiviazione chiavi per smart card per l'attestazione della chiave, mentre i dispositivi non appartenenti al dominio possono ora usare la registrazione NDES per ottenere i certificati che possono essere attestati per le chiavi in un TPM.  

### Active Directory Domain Services  
Active Directory Domain Services include miglioramenti che consentono alle organizzazioni di proteggere gli ambienti Active Directory e fornire una migliore esperienza nella gestione delle identità per i dispositivi aziendali e personali. Per altre informazioni, vedere [What's new in Active Directory Domain Services (AD DS) in WindowsServer 2016](../identity/whats-new-active-directory-domain-services.md) (Novità di Active Directory Domain Services (AD DS) in WindowsServer 2016).   

### Active Directory Federation Services  
Novità di Active Directory Federation Services. Active Directory Federation Services (AD FS) in WindowsServer 2016 include nuove funzionalità che consentono di configurare AD FS per l'autenticazione degli utenti archiviati nelle directory LDAP (Lightweight Directory Access Protocol). Per altre informazioni, vedi [Novità di AD FS per WindowsServer 2016](../identity/ad-fs/overview/whats-new-active-directory-federation-services-windows-server.md).  

### Proxy applicazione Web  
La versione più recente di Proxy applicazione Web è incentrata sulle nuove funzionalità che consentono la pubblicazione e la preautenticazione per più applicazioni e un'esperienza utente migliorata. Consultare l'elenco completo delle nuove funzionalità che include la preautenticazione per applicazioni rich client quali domini di Exchange ActiveSync e con caratteri jolly per la pubblicazione più semplice di applicazioni SharePoint. Per altre informazioni, vedi [Proxy applicazione Web in Windows Server 2016](../remote/remote-access/web-application-proxy/web-application-proxy-windows-server.md).  

##  [Amministrazione](../administration/manage-windows-server.md)  
L'area Gestione e automazione è incentrata sulle informazioni di riferimento e degli strumenti per i professionisti IT che vogliono eseguire e gestire WindowsServer 2016, incluso Windows PowerShell.

Windows PowerShell 5.1 include nuove funzionalità significative, tra cui il supporto per lo sviluppo con le classi e nuove funzionalità di sicurezza che ne estendono e migliorano l'uso, oltre a consentire di controllare e gestire gli ambienti Windows in modo più semplice e completo. Per dettagli, vedi [Nuovi scenari e funzionalità in WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/scenarios-features).

Le nuove funzionalità aggiunte per WindowsServer 2016 includono la possibilità di eseguire PowerShell.exe in locale su Nano Server (non più solo in remoto), i nuovi cmdlet per utenti e gruppi locali che sostituiscono l'interfaccia utente grafica, il supporto per il debug di PowerShell e il supporto per la registrazione di protezione, la trascrizione e la tecnologia JEA (Just Enough Administration) in Nano Server.

Ecco alcune delle altre nuove funzionalità di amministrazione:

### Desired State Configuration (DSC) di PowerShell in Windows Management Framework (WMF) 5
Windows Management Framework 5 include gli aggiornamenti per Configurazione dello stato desiderato tramite PowerShell, Gestione remota Windows (WinRM) e Strumentazione gestione Windows (WMI).

Per altre informazioni sull'esecuzione dei test delle funzionalità Desired State Configuration (DSC) di Windows Management Framework 5, vedi la serie di post di blog descritti nella pagina relativa alla [convalida delle funzionalità Desired State Configuration di PowerShell](https://blogs.msdn.microsoft.com/powershell/2015/07/06/validate-features-of-powershell-dsc/). Per il download, vedi [Windows Management Framework 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

### Gestione unificata dei pacchetti PackageManagement per l'individuazione, l'installazione e l'inventario di prodotti software
Windows Server 2016 e Windows 10 includono una nuova funzionalità PackageManagement (denominata in precedenza OneGet) che consente ai professionisti IT o DevOps di automatizzare l'individuazione, l'installazione e l'inventario dei prodotti software, localmente o in remoto, indipendentemente dalla tecnologia di installazione e dalla posizione del software. 

Per altre info, vedi [https://github.com/OneGet/oneget/wiki](https://github.com/OneGet/oneget/wiki).

### Miglioramenti di PowerShell a supporto dell'informatica forense e per consentire di ridurre le violazioni della sicurezza
Per aiutare il team responsabile dell'analisi dei sistemi compromessi, sono state aggiunte a PowerShell opzioni di registrazione e di informatica forense e sono state aggiunte funzionalità per ridurre le vulnerabilità degli script, ad esempio API vincolate di PowerShell e API CodeGeneration sicure.

Per altre informazioni, vedi [PowerShell ♥ the Blue Team](https://blogs.msdn.microsoft.com/powershell/2015/06/09/powershell-the-blue-team/).

## [Reti](../networking/Networking.md)  
Questa area si focalizza sui prodotti e sulle funzionalità di rete utilizzabili dai professionisti IT per la progettazione, la distribuzione e la manutenzione di WindowsServer 2016.  

### rete definita tramite software
È ora possibile eseguire il mirroring e instradare il traffico verso appliance virtuali nuove o esistenti. Insieme a un firewall distribuito e ai gruppi di sicurezza di rete, consente di effettuare la segmentazione in modo dinamico e proteggere i carichi di lavoro in modo analogo ad Azure. In secondo luogo, è possibile distribuire e gestire tutto lo stack di SDN (Software Defined Networking) utilizzando System Center Virtual Machine Manager. Infine, è possibile usare Docker per gestire la rete di contenitori di WindowsServer e associare i criteri di SDN non solo alle macchine virtuali ma anche ai contenitori. Per altre informazioni, vedi [Pianificare un'infrastruttura Software Defined Network](../networking/sdn/plan/plan-a-software-defined-network-infrastructure.md).

### Miglioramenti delle prestazioni delle connessioni TCP
Il valore predefinito di ICW (Initial Congestion Window) è stato aumentato da 4 a 10 ed è stato implementato TFO (TCP Fast Open). TFO riduce la quantità di tempo necessaria per stabilire una connessione TCP e il valore incrementato di ICW consente il trasferimento di oggetti di maggiori dimensioni nel burst iniziale. Questa combinazione può ridurre notevolmente il tempo necessario per trasferire un oggetto Internet tra il client e il cloud.

Per migliorare il comportamento di TCP durante il ripristino da una perdita di pacchetti sono state implementate le funzionalità TLP (TCP Tail Loss Probe) e RACK (Recent Acknowledgement). TLP consente di convertire il timeout di ritrasmissione (RTO) in ripristini veloci e RACK riduce il tempo necessario per il ripristino rapido per ritrasmettere un pacchetto perso. 

## [Sicurezza e controllo](../security/Security-and-Assurance.md)  
Include funzionalità e soluzioni di protezione per il professionista IT che esegue la distribuzione nell'ambiente cloud e del centro dati. Per informazioni generali sulla sicurezza in WindowsServer 2016, vedere [Sicurezza e controllo](../security/Security-and-Assurance.md).  

### JEA  
JEA (Just Enough Administration) in WindowsServer 2016 è una tecnologia di protezione che consente l'amministrazione delegata per qualsiasi elemento che può essere gestito con Windows PowerShell. Le funzionalità includono il supporto per l'esecuzione in un'identità di rete, la connessione tramite PowerShell Direct, la copia sicura dei file da o verso gli endpoint JEA e la configurazione della console di PowerShell in modo da poterla avviare per impostazione predefinita in un contesto JEA. Per altri dettagli, vedi [JEA in GitHub](https://aka.ms/JEA).

### Credential Guard
Credential Guard usa la protezione basata su virtualizzazione per isolare i segreti in modo che solo il software di sistema con privilegi possa accedervi. Vedere [Proteggere le credenziali di dominio derivate con Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard).

###  Remote Credential Guard
Credential Guard include il supporto per le sessioni RDP in modo che le credenziali utente rimangano sul lato client e non siano esposte sul lato server. Include inoltre la funzionalità Single Sign-On per Desktop remoto. Vedi [Proteggere le credenziali di dominio derivate con Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).   

### Device Guard (integrità del codice)
Device Guard garantisce l'integrità del codice in modalità kernel (KMCI, Kernel Mode Code Integrity) e in modalità utente (UMCI, User Mode Code Integrity) mediante la creazione di criteri che specificano quale codice può essere eseguito sul server. Vedi [Introduzione a Windows Defender Device Guard: sicurezza basata su virtualizzazione e criteri di integrità del codice](https://docs.microsoft.com/windows/device-security/device-guard/introduction-to-device-guard-virtualization-based-security-and-code-integrity-policies).


### Windows Defender  
[Panoramica di Windows Defender per WindowsServer 2016](../security/windows-defender/windows-defender-overview-windows-server.md). WindowsServer Antimalware è installato e abilitato per impostazione predefinita in WindowsServer 2016, ma l'interfaccia utente per WindowsServer Antimalware non è installata. Tuttavia, WindowsServer Antimalware aggiorna le definizioni antimalware e protegge il computer senza l'interfaccia utente. Se è necessaria l'interfaccia utente per WindowsServer Antimalware, è possibile installarla dopo l'installazione del sistema operativo usando l'aggiunta guidata ruoli e funzionalità.

### Protezione del flusso di controllo
Questa funzionalità di protezione della piattaforma è stata creata per contrastare le vulnerabilità legate al danneggiamento della memoria. Per altre informazioni, vedere [Control Flow Guard](https://msdn.microsoft.com/library/windows/desktop/mt637065(v=vs.85).aspx) (Protezione del flusso di controllo).


## [Archiviazione](../storage/storage.md)

In WindowsServer 2016 la tecnologia di archiviazione include nuove funzionalità e miglioramenti per l'archiviazione software-defined e anche per i file server tradizionali. Di seguito sono riportate alcune delle nuove funzionalità. Per informazioni su altri miglioramenti e per altri dettagli, vedere [Novità di Archiviazione in WindowsServer 2016](../storage/whats-new-in-storage.md).

### Spazi di archiviazione diretta

Spazi di archiviazione diretta consente di creare un'archiviazione altamente disponibile e scalabile con server di archiviazione locale. Questa funzionalità semplifica la distribuzione e la gestione dei sistemi con archiviazione definita dal software e consente l'uso di nuove classi di dispositivi disco, ad esempio le unità SSD SATA e NVMe, che in precedenza non erano disponibili con spazi di archiviazione raggruppati in cluster che includevano i dischi condivisi.

Per altre informazioni, vedere [Spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-overview.md).

### Replica archiviazione

Replica di archiviazione consente di eseguire la replica sincrona a livello di blocco, indipendente dall'archiviazione, tra server o cluster per il ripristino di emergenza, nonché l'estensione di un cluster di failover tra siti. La replica sincrona consente il mirroring dei dati in siti fisici con volumi coerenti per arresto anomalo del sistema senza perdere dati a livello di file system. La replica asincrona consente l'estensione del sito oltre le aree metropolitane con la possibilità di perdita di dati.

Per altre informazioni, vedere [Replica di archiviazione](../storage/storage-replica/storage-replica-overview.md).

### QoS di archiviazione

È ora possibile usare QoS di archiviazione per monitorare in modo centralizzato le prestazioni di archiviazione end-to-end e creare criteri di gestione usando cluster Hyper-V e CSV in WindowsServer 2016.

Per altre informazioni, vedere [Storage Quality of Service](../storage/storage-qos/storage-qos-overview.md) (QoS di archiviazione).

## [Clustering di failover](../failover-clustering/whats-new-in-failover-clustering.md)

WindowsServer 2016 include una serie di nuove funzionalità e miglioramenti per più server raggruppati in un unico cluster a tolleranza di errore mediante la funzione Clustering di failover. Alcune delle funzionalità aggiunte sono elencate di seguito. Per un elenco completo, vedere [Novità del clustering di failover in WindowsServer 2016](../failover-clustering/whats-new-in-failover-clustering.md).

### Aggiornamento in sequenza del sistema operativo del cluster

L'aggiornamento in sequenza del sistema operativo del cluster consente a un amministratore di aggiornare il sistema operativo dei nodi del cluster da WindowsServer 2012 R2 a WindowsServer 2016 senza interrompere i carichi di lavoro del file server di scalabilità orizzontale o di Hyper-V. Usando questa funzionalità, è possibile evitare le sanzioni per il tempo di inattività previste dai contratti di servizio.

Per altre informazioni, vedere [Aggiornamento in sequenza del sistema operativo del cluster](../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md).

### Cloud di controllo

Cloud di controllo è un nuovo tipo di quorum di controllo per un cluster di failover in WindowsServer 2016 che si basa su Microsoft Azure come punto di arbitraggio. Cloud di controllo, come altri quorum di controllo, ottiene un voto e può partecipare ai calcoli del quorum. È possibile configurare questa funzionalità come quorum di controllo usando la Configurazione guidata quorum del cluster.

Per altre informazioni, vedere [Deploy Cloud Witness](../failover-clustering/deploy-cloud-witness.md) (Distribuire un cloud di controllo).

### Servizio integrità

Il Servizio integrità migliora le attività giornaliere di monitoraggio, esecuzione e manutenzione delle risorse in un cluster di Spazi di archiviazione diretta.

Per altre informazioni, vedi [Servizio integrità](../failover-clustering/health-service-overview.md).

## Sviluppo di applicazioni

### Internet Information Services (IIS) 10.0
Le nuove funzionalità fornite dal server Web IIS 10.0 in Windows Server 2016 includono:

- Supporto per il protocollo HTTP/2 nello stack di rete e integrato con IIS 10.0, consentendo ai siti Web IIS 10.0 di servire automaticamente le richieste HTTP/2 per le configurazioni supportate. Ciò consente numerosi miglioramenti rispetto a HTTP/1.1, ad esempio il più efficiente riutilizzo delle connessioni e la ridotta latenza, migliorando i tempi di caricamento delle pagine Web. 
- Possibilità di eseguire e gestire IIS 10.0 in Nano Server. Vedi [IIS in Nano Server](iis-on-nano-server.md).
- Supporto per le intestazioni Host con caratteri jolly, consentendo agli amministratori di configurare un server web per un dominio e quindi impostare il server web le richieste di qualsiasi sottodominio.
- Un nuovo modulo di PowerShell (IISAdministration) per la gestione di IIS. 

Per ulteriori dettagli, vedi [IIS](https://iis.net/learn).

### Distributed Transaction Coordinator (MSDTC)
Tre nuove funzionalità sono state aggiunte in Microsoft Windows 10 e Windows Server 2016:

- Una nuova interfaccia per la funzione di riaggiunta di Gestione risorse è utilizzabile da un gestore delle risorse per determinare il risultato di una transazione in dubbio dopo il riavvio di un database a causa di un errore. Vedi [IResourceManagerRejoinable::Rejoin](https://msdn.microsoft.com/en-us/library/mt203799(v=vs.85).aspx) per informazioni dettagliate.

- Il limite per i nomi DSN viene aumentato da 256 byte a 3072 byte. Per i dettagli, vedi [IDtcToXaHelperFactory::Create](https://msdn.microsoft.com/en-us/library/ms686861(v=vs.85).aspx), [IDtcToXaHelperSinglePipe::XARMCreate](https://msdn.microsoft.com/en-us/library/ms679248(v=vs.85).aspx) o [IDtcToXaMapper::RequestNewResourceManager](https://msdn.microsoft.com/en-us/library/ms680310(v=vs.85).aspx).

- La migliorata funzione di traccia ti consente di impostare una chiave del Registro di sistema per includere un percorso di file di immagine nel nome del file di log di traccia in modo da indicare quale file di log di traccia controllare. Vedi [Come abilitare la traccia di diagnostica per MS DTC in un computer basato su Windows](https://support.microsoft.com/en-us/kb/926099) per informazioni dettagliate sulla configurazione della traccia per MSDTC.



## Vedi anche  
-   [Note sulla versione: problemi importanti di Windows Server 2016](Windows-Server-2016-GA-Release-Notes.md)  

