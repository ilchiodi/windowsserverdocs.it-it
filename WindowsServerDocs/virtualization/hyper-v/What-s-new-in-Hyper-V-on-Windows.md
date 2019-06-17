---
title: Novità in Hyper-V in Windows Server 2016
description: Offre un riepilogo delle nuove funzionalità di Hyper-V
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1a65a98e-54b6-4c41-9732-1e3d32fe3a5f
author: KBDAzure
ms.author: kathydav
ms.date: 09/21/2017
ms.openlocfilehash: 8b7d9233b105f710d620b5142205fb2eadd0248a
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/14/2019
ms.locfileid: "67141367"
---
# <a name="whats-new-in-hyper-v-on-windows-server"></a>Quali sono le novità in Hyper-V in Windows Server

>Si applica a: Windows Server 2019, Microsoft Hyper-V Server 2016, Windows Server 2016
  
Questo articolo illustra le funzionalità nuove e modificate di Hyper-V in Microsoft Hyper-V Server 2016, Windows Server 2016 e Windows Server 2019. Per usare le nuove funzionalità nelle macchine virtuali create con Windows Server 2012 R2 e spostati o importato in un server che esegue Hyper-V in Windows Server 2019 o Windows Server 2016, è necessario aggiornare manualmente la versione di configurazione macchina virtuale. Per istruzioni, vedere [versione aggiornamento macchina virtuale](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md).  
  
Ecco cosa è incluso in questo articolo e se la funzionalità è nuovo o aggiornato.  

## <a name="windows-server-version-1903"></a>Windows Server, versione 1903

### <a name="add-hyper-v-manager-to-server-core-installations-updated"></a>Aggiungere Hyper-V Manager per le installazioni Server Core (aggiornate)

Come sapete, è consigliabile usare l'opzione di installazione Server Core, quando si usa Windows Server, come canale semestrale in fase di produzione. Server Core per impostazione predefinita, tuttavia, omette una serie di strumenti di gestione utile. È possibile aggiungere molti più comunemente usati strumenti installando la funzionalità di compatibilità dell'applicazione, ma sono ancora state alcuni strumenti mancante.

In base ai suggerimenti dei clienti, Aggiungemmo uno più strumenti per la funzionalità di compatibilità dell'applicazione in questa versione: Gestione di Hyper-V (virtmgmt.msc).

Per altre informazioni, vedi [funzionalità di compatibilità di base del Server app](../../get-started-19/install-fod-19.md).

## <a name="windows-server-2019"></a>Windows Server 2019

### <a name="security-shielded-virtual-machines-improvements-new"></a>Sicurezza: Miglioramenti a macchine virtuali schermati (nuovi)

- **Miglioramenti di branch office**

    Ora puoi eseguire macchine virtuali schermate in computer con connettività intermittente al servizio Sorveglianza host sfruttando le nuove funzionalità del [server HGS di fallback](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#fallback-configuration) e la [modalità offline](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#offline-mode). Il server HGS di fallback ti consente di configurare un secondo gruppo di URL per Hyper-V per provare se è in grado di raggiungere il server HGS primario.

    La modalità offline ti consente di continuare ad avviare le macchine virtuali schermate, anche se HGS non può essere raggiunto, purché la macchina virtuale sia stata avviata correttamente una volta e la configurazione di protezione dell'host non sia cambiata.

- **Miglioramenti di risoluzione dei problemi**

    Abbiamo anche semplificato la [risoluzione dei problemi delle macchine virtuali schermate](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms) abilitando il supporto per la modalità sessione avanzata VMConnect e PowerShell Direct. Questi strumenti sono particolarmente utili se hai perso la connettività di rete alla macchina virtuale e devi aggiornarne la configurazione per ripristinare l'accesso.

    Queste funzionalità non devono essere configurate e vengono rese disponibili automaticamente quando una macchina virtuale schermata viene posizionata su un host Hyper-V che esegue Windows Server versione 1803 o successiva.

- **Supporto Linux**

    Se esegui ambienti di sistema operativo misto, adesso Windows Server 2019 supporta l'esecuzione di Ubuntu, Red Hat Enterprise Linux e SUSE Linux Enterprise Server in macchine virtuali schermate.

## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="compatible-with-connected-standby-new"></a>Compatibile con Standby connesso \(new\)

Quando è installato il ruolo Hyper-V in un computer che utilizza il modello di risparmio energia sempre On/Always connessi (AOAC), il **Standby connesso** stato di alimentazione è ora disponibile.  
  
### <a name="discrete-device-assignment-new"></a>Assegnazione dispositivo discreti \(new\)

Questa funzionalità consente di fornire accesso diretto ed esclusivo di una macchina virtuale per alcuni dispositivi hardware PCIe. Utilizzo di un dispositivo in questo modo consente di ignorare lo stack di virtualizzazione Hyper-V, che comporta un accesso più rapido. Per informazioni dettagliate sull'hardware supportato, vedere "assegnazione discreti dispositivo" in [requisiti di sistema per Hyper-V in Windows Server 2016](System-requirements-for-Hyper-V-on-Windows.md). Per altre informazioni, nonché come utilizzare questa funzionalità e considerazioni, vedere il post "[discreti dispositivo assegnazione-descrizione e lo sfondo](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/)" nel blog di virtualizzazione.

### <a name="encryption-support-for-the-operating-system-disk-in-generation-1-virtual-machines-new"></a>Supporto della crittografia per il disco del sistema operativo nelle macchine virtuali di prima generazione 1 \(nuovi)

È ora possibile proteggere il disco del sistema operativo mediante crittografia unità BitLocker in macchine virtuali di generazione 1. Una nuova funzionalità, l'archiviazione delle chiavi, crea un'unità di piccole dimensioni, dedicata per archiviare la chiave di BitLocker dell'unità di sistema. Questa operazione viene eseguita invece di utilizzare un virtuale modulo TPM (Trusted Platform), che è disponibile solo in macchine virtuali di generazione 2. Per decrittografare il disco e avviare la macchina virtuale, l'host Hyper-V deve far parte di un'infrastruttura protetta autorizzata o dispone della chiave privata da uno dei tutori della macchina virtuale. Archiviazione delle chiavi richiede una macchina virtuale versione 8. Per informazioni sulla versione di macchina virtuale, vedere [versione aggiornamento macchina virtuale in Hyper-V in Windows 10 o Windows Server 2016](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md).  
  
### <a name="host-resource-protection-new"></a>Protezione delle risorse di ospitare \(new\)

Questa funzionalità consente di evitare che una macchina virtuale utilizza più di relativa condivisione di risorse di sistema mediante la ricerca di livelli di un numero eccessivo di attività. Questo può impedire attività eccessiva della macchina virtuale di peggiorare le prestazioni di host o altre macchine virtuali. Quando il monitoraggio rileva una macchina virtuale con un eccesso di attività, la macchina virtuale viene assegnata meno risorse. Questo monitoraggio e l'applicazione è disattivata per impostazione predefinita. Utilizzare Windows PowerShell per attiva o disattivata. Per attivarla, eseguire questo comando:  
  
```
Set-VMProcessor TestVM -EnableHostResourceProtection $true
```

Per informazioni dettagliate su questo cmdlet, vedere [Set-VMProcessor](https://docs.microsoft.com/powershell/module/hyper-v/set-vmprocessor).

### <a name="hot-add-and-remove-for-network-adapters-and-memory-new"></a>Aggiungere e rimuovere schede di rete e memoria a caldo \(new\)

È ora possibile aggiungere o rimuovere una scheda di rete, mentre la macchina virtuale è in esecuzione, senza ciò potrebbe comportare tempi di inattività. Questa procedura funziona per le macchine virtuali di generazione 2 che eseguono sistemi operativi Windows o Linux.  
  
È inoltre possibile regolare la quantità di memoria assegnata a una macchina virtuale mentre è in esecuzione, anche se non è stata abilitata la memoria dinamica. Ciò avviene per la generazione 1 e macchine virtuali di generazione 2, che esegue Windows Server 2016 o Windows 10.  

### <a name="hyper-v-manager-improvements-updated"></a>Miglioramenti di gestione di Hyper-V \(aggiornato\) 
  
-   **Supporto di credenziali alternative** -è ora possibile utilizzare un diverso set di credenziali di gestione di Hyper-V quando ci si connette a un altro host remoto Windows 10 o Windows Server 2016. È inoltre possibile salvare le credenziali per rendere più semplice di accedere di nuovo.  
  
-   **Gestire le versioni precedenti** -con gestione di Hyper-V in Windows Server 2019, Windows Server 2016 e Windows 10, è possibile gestire i computer che eseguono Hyper-V in Windows Server 2012, Windows 8, Windows Server 2012 R2 e Windows 8.1.  
  
-   **Protocollo di gestione aggiornato** -Hyper-V Manager ora comunica con gli host Hyper-V remoti utilizzando il protocollo WS-MANAGEMENT, che consente l'autenticazione CredSSP, Kerberos o NTLM. Quando si utilizza CredSSP per connettersi a un host Hyper-V remoto, è possibile eseguire una migrazione in tempo reale senza abilitare la delega vincolata in Active Directory. L'infrastruttura basate su WS-MAN rende anche più semplice attivare un host per la gestione remota. WS-Management effettua la connessione tramite la porta 80, aperta per impostazione predefinita.  
  
### <a name="integration-services-delivered-through-windows-update-updated"></a>Servizi di integrazione forniti tramite Windows Update \(aggiornato\) 

Gli aggiornamenti a integration services per gli ospiti di Windows vengono distribuiti tramite Windows Update. Per i provider di servizi e hoster cloud privato, in questo modo il controllo di applicazione degli aggiornamenti nelle mani di tenant che possiedono le macchine virtuali. Tenant ora possono aggiornare le proprie macchine virtuali di Windows con tutti gli aggiornamenti, inclusi i servizi di integrazione, utilizzando un singolo metodo. Per informazioni dettagliate sui servizi di integrazione per i Guest Linux, vedere [Linux e FreeBSD le macchine virtuali in Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).  
  
> [!IMPORTANT]  
> Il file di immagine vmguest non è più necessario, pertanto, non è incluso con Hyper-V in Windows Server 2016.  
  
### <a name="linux-secure-boot-new"></a>Avvio protetto Linux \(new\) 

Sistemi operativi Linux in esecuzione su macchine virtuali di generazione 2 possono ora di avvio con l'opzione di avvio protetto abilitata. Ubuntu 14.04 e versioni successive, SUSE Linux Enterprise Server 12 e successive, Red Hat Enterprise Linux 7.0 e versioni successive e CentOS 7.0 e versioni successive sono abilitati per l'avvio protetto sugli host che eseguono Windows Server 2016. Prima avviare la macchina virtuale per la prima volta, è necessario configurare la macchina virtuale per l'utilizzo di autorità di certificazione Microsoft UEFI. È possibile farlo dalla gestione di Hyper-V, Virtual Machine Manager o una sessione di Windows Powershell con privilegi elevata. Per Windows PowerShell, eseguire questo comando:  
  
```  
Set-VMFirmware TestVM -SecureBootTemplate MicrosoftUEFICertificateAuthority  
```  
  
Per ulteriori informazioni sulle macchine virtuali Linux in Hyper-V, vedere [Linux e FreeBSD le macchine virtuali in Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md). Per ulteriori informazioni sul cmdlet, vedere [Set-VMFirmware](https://docs.microsoft.com/powershell/module/hyper-v/set-vmfirmware).

### <a name="more-memory-and-processors-for-generation-2-virtual-machines-and-hyper-v-hosts-updated"></a>Più memoria e processori per gli host Hyper-V e macchine virtuali di generazione 2 \(aggiornato\)

A partire dalla versione 8, macchine virtuali di generazione 2 possono utilizzare molte più memoria e processori virtuali. Host inoltre possono essere configurati con molte più memoria e processori virtuali che erano supportati. Queste modifiche supportano nuovi scenari quali l'esecuzione di database di grandi dimensioni in memoria per l'elaborazione delle transazioni online (OLTP) e di data warehousing (DW) e-commerce. Blog di Windows Server recentemente pubblicato i risultati delle prestazioni di una macchina virtuale con 5.5 terabyte di memoria e 128 processori virtuali in esecuzione di database in memoria di 4 TB. Le prestazioni era maggiore del 95% delle prestazioni di un server fisico. Per informazioni dettagliate, vedere [prestazioni della macchina Virtuale su larga scala di Windows Server 2016 Hyper-V per l'elaborazione delle transazioni in memoria](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/). Per informazioni dettagliate sulle versioni di macchina virtuale, vedere [versione aggiornamento macchina virtuale in Hyper-V in Windows 10 o Windows Server 2016](./deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Per l'elenco completo delle configurazioni massime supportate, vedere [pianificare la scalabilità di Hyper-V in Windows Server 2016](./plan/plan-hyper-v-scalability-in-windows-server.md). 

### <a name="nested-virtualization-new"></a>La virtualizzazione annidata \(new\)

Questa funzionalità consente di utilizzare una macchina virtuale come host Hyper-V e creare macchine virtuali all'interno di tale host virtualizzato. Ciò può risultare particolarmente utile per ambienti di sviluppo e test. Per utilizzare la virtualizzazione nidificata, è necessario:  
  
-   Per eseguire almeno Windows Server 2019, Windows Server 2016 o Windows 10 in host Hyper-V fisici e l'host virtualizzato.  
  
-   Un processore con Intel VT-x (virtualizzazione nidificata è disponibile solo per i processori Intel in questo momento).  
  
Per informazioni dettagliate e istruzioni, vedere [eseguire Hyper-V in una macchina virtuale con la virtualizzazione nidificata](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization).  
  
### <a name="networking-features-new"></a>Funzionalità di rete \(new\)

Nuove funzionalità di rete includono:  
  
-   **Remote diretto alla memoria (RDMA) e passare gruppo incorporato (SET)** . È possibile impostare RDMA sulle schede di rete associate a un commutatore virtuale Hyper-V, indipendentemente dal fatto che SET viene anche utilizzato. SET di fornisce un commutatore virtuale con alcune delle stesse funzionalità gruppo NIC. Per informazioni dettagliate, vedere [diretta accesso memoria remota (RDMA) e Switch incorporati di raggruppamento (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).  
  
-   **Macchina virtuale più code (VMMQ)** . Migliora la velocità effettiva VMQ allocando più code di hardware per ogni macchina virtuale.  La coda predefinita diventa un insieme di code per una macchina virtuale e il traffico viene distribuito tra le code.  
  
-   **Qualità del servizio (QoS) per le reti definita dal software**. Gestisce la classe predefinita del traffico attraverso il commutatore virtuale all'interno della larghezza di banda di classe predefinito.  
  
Per ulteriori informazioni sulle nuove funzionalità di rete, vedere [What's new in rete](../../networking/What-s-New-in-Networking.md).  
  
### <a name="production-checkpoints-new"></a>I checkpoint di produzione \(new\)

I checkpoint di produzione sono immagini "point-in-time" di una macchina virtuale. In tal modo, è un modo per applicare un checkpoint che è conforme ai criteri di supporto quando una macchina virtuale in esecuzione un carico di lavoro di produzione. I checkpoint di produzione sono basati sulla tecnologia di backup nella macchina guest invece di uno stato salvato. Per le macchine virtuali Windows, viene utilizzato il servizio Snapshot Volume (VSS). Per le macchine virtuali Linux, verranno scaricati i buffer di file system per creare un checkpoint che è coerente con il file system. Se si desidera utilizzare i checkpoint in base agli stati salvati, scegliere i checkpoint standard. Per informazioni dettagliate, vedere [puoi scegliere tra checkpoint standard o di produzione in Hyper-V](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md).  
  
> [!IMPORTANT]  
> Nuove macchine virtuali utilizzarli produzione come impostazione predefinita.  
  
### <a name="rolling-hyper-v-cluster-upgrade-new"></a>Aggiornamento di Cluster Hyper-V in sequenza \(new\)

È ora possibile aggiungere un nodo che esegue Windows Server 2016 o Windows Server 2019 per un Cluster Hyper-V con nodi che eseguono Windows Server 2012 R2. Ciò consente di aggiornare il cluster senza tempi di inattività. Il cluster viene eseguito a un livello di funzionalità di Windows Server 2012 R2 finché non si aggiorna tutti i nodi del cluster e aggiorna il livello di funzionalità cluster con il cmdlet Windows PowerShell, [ClusterFunctionalLevel aggiornamento](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel).  
  
> [!IMPORTANT]  
> Dopo aver aggiornato il livello di funzionalità del cluster, è possibile restituire, a Windows Server 2012 R2.  
  
Per un cluster Hyper-V con un livello di funzionalità di Windows Server 2012 R2 con nodi che eseguono Windows Server 2012 R2, Windows Server 2019 e Windows Server 2016, tenere presente quanto segue:  
  
-   Gestire il cluster, Hyper-V e macchine virtuali da un nodo che esegue Windows Server 2016 o Windows 10.  
  
-   È possibile spostare le macchine virtuali tra tutti i nodi del cluster Hyper-V.  
  
-   Per utilizzare nuove funzionalità di Hyper-V, tutti i nodi devono eseguire Windows Server 2016 o il livello funzionale del cluster deve essere aggiornata.  
  
-   La versione di configurazione macchina virtuale per le macchine virtuali esistenti non è aggiornata. È possibile aggiornare la versione di configurazione solo dopo l'aggiornamento a livello funzionale del cluster.  
  
-   Le macchine virtuali create sono compatibili con Windows Server 2012 R2, il livello di configurazione macchina virtuale 5.  
  
Dopo l'aggiornamento a livello funzionale del cluster:  
  
-   È possibile abilitare le nuove funzionalità di Hyper-V.  
  
-   Per rendere disponibili nuove funzionalità di macchina virtuale, utilizzare il cmdlet Update-VmConfigurationVersion per aggiornare manualmente il livello di configurazione macchina virtuale. Per istruzioni, vedere [versione aggiornamento macchina virtuale](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md).    
-   È possibile aggiungere un nodo al Cluster Hyper-V che esegue Windows Server 2012 R2.  
  
> [!NOTE]  
> Hyper-V in Windows 10 non supporta il clustering di failover.  
  
Per informazioni dettagliate e istruzioni, vedere il [aggiornamento in sequenza di Cluster del sistema operativo](https://technet.microsoft.com/library/dn850430.aspx).  

### <a name="shared-virtual-hard-disks-updated"></a>Dischi rigidi virtuali condivisi \(aggiornato\)
È ora possibile ridimensionare i dischi rigidi virtuali condivisi (vhdx) utilizzati per il clustering, senza tempi di inattività guest. Dischi rigidi virtuali condivisi può essere aumentati o ridotti mentre la macchina virtuale è in linea. Cluster guest ora possono inoltre proteggere dischi rigidi virtuali condivisi tramite Replica Hyper-V per il ripristino di emergenza.

Abilitare la replica per la raccolta. Abilitazione della replica in una raccolta è **esposti solo tramite l'interfaccia WMI**. Vedere la documentazione relativa [Msvm_CollectionReplicationService classe](https://msdn.microsoft.com/library/mt167787%28v=vs.85%29.aspx) per altri dettagli. **Non è possibile gestire la replica di una raccolta tramite cmdlet di PowerShell o l'interfaccia utente.** Le macchine virtuali devono trovarsi negli host che fanno parte di un cluster Hyper-V per accedere alle funzionalità specifiche di una raccolta. Disco rigido virtuale condiviso sono inclusi: i dischi rigidi virtuali condivisi in host autonomi non sono supportati dalla Replica Hyper-V.

Seguire le linee guida per i dischi rigidi virtuali condivisi nelle [Virtual Hard Disk Sharing Overview](https://technet.microsoft.com/library/dn281956.aspx)e assicurarsi che i dischi rigidi virtuali condivisi sono parte di un cluster guest. 

Una raccolta con un disco rigido virtuale condiviso, ma non del cluster guest associato non è possibile creare i punti di riferimento per la raccolta (indipendentemente dal fatto che il disco rigido virtuale condiviso è incluso nella creazione del punto di riferimento o non). 

### <a name="virtual-machine-backupnew"></a>Backup delle macchine virtuali\(new\)

Se esegue il backup di una singola macchina virtuale (indipendentemente dal fatto che host è di tipo cluster oppure No), non si utilizzino un gruppo di macchine Virtuali.  Non è consigliabile usare una raccolta di snapshot. I gruppi VM e la raccolta di snapshot sono concepite per essere utilizzate esclusivamente per il backup dei cluster guest che Usa file vhdx condiviso. In alternativa, è necessario fare uno snapshot usando il [provider WMI di Hyper-V v2](https://msdn.microsoft.com/library/windows/desktop/hh850319(v=vs.85).aspx). Analogamente, non usare la [provider WMI di Cluster di Failover](https://msdn.microsoft.com/library/windows/desktop/mt167750(v=vs.85).aspx).

### <a name="shielded-virtual-machines-new"></a>Le macchine virtuali schermate \(new\)

Schermatura utilizzare macchine virtuali diverse funzionalità che rendono più difficile per gli amministratori di Hyper-V e malware nell'host controllare, alterare o rubare i dati dallo stato di una macchina virtuale schermata. Dati e lo stato è crittografato, gli amministratori di Hyper-V non è possibile visualizzare l'output video e i dischi e le macchine virtuali possono essere limitate per l'esecuzione solo su host integro noto, come determinato da un Server di sorveglianza Host. Per informazioni dettagliate, vedere [infrastruttura protetta e macchine virtuali schermati](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md).
  
> [!NOTE]  
> Le macchine virtuali schermate sono compatibili con la Replica Hyper-V. Per replicare una macchina virtuale schermata, l'host che si desidera replicare deve essere autorizzato a eseguire tale macchina virtuale schermata.  

### <a name="start-order-priority-for-clustered-virtual-machines-new"></a>Avviare la priorità di ordine per macchine virtuali in cluster \(new\)

Questa funzionalità offre maggiore controllo sulla quale macchine virtuali del cluster viene avviate o riavviate prima. Questo rende più semplice avviare le macchine virtuali che forniscono servizi prima delle macchine virtuali che utilizzano tali servizi. Definire i set, posizionare le macchine virtuali nei set e specificare le dipendenze. Usare i cmdlet di Windows PowerShell per gestire i set, ad esempio [New-ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/new-clustergroupset), [Get-ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustergroupset), e [Clustergroupsetdependency Aggiungi](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergroupsetdependency).
.  
### <a name="storage-quality-of-service-qos-updated"></a>Qualità del servizio (QoS) di archiviazione \(aggiornato\)

È ora possibile creare criteri QoS di archiviazione in un File server di scalabilità orizzontale e assegnarli a uno o più dischi virtuali in macchine virtuali Hyper-V. Le prestazioni di archiviazione vengono regolate automaticamente in modo da soddisfare i criteri man mano che cambia il carico di archiviazione. Per informazioni dettagliate, vedere [qualità del servizio di archiviazione](../../storage/storage-qos/storage-qos-overview.md).  
  
### <a name="virtual-machine-configuration-file-format-updated"></a>Formato di file di configurazione macchina virtuale \(aggiornato\)

File di configurazione macchina virtuale utilizzano un nuovo formato di operazioni di lettura e scrittura dei dati di configurazione più efficiente. Il formato rende inoltre il danneggiamento dei dati meno probabile che se si verifica un errore di archiviazione. File di dati di configurazione macchina virtuale utilizzano un'estensione di nome file .vmcx e file di dati dello stato di runtime utilizzano un'estensione di nome file .vmrs.  
  
> [!IMPORTANT]  
> L'estensione del nome file .vmcx indica un file binario. Modifica dei file .vmcx o .vmrs non è supportata.  
  
### <a name="virtual-machine-configuration-version-updated"></a>Versione di configurazione della macchina virtuale \(aggiornato\)

La versione rappresenta la compatibilità di configurazione della macchina virtuale, salvata lo stato e i file di snapshot con la versione di Hyper-V. Le macchine virtuali con versione 5 sono compatibili con Windows Server 2012 R2 e può eseguire in Windows Server 2012 R2 sia Windows Server 2016. Le macchine virtuali con le versioni introdotte in Windows Server 2016 ed e Windows Server 2019 non verrà eseguito in Hyper-V in Windows Server 2012 R2.   
  
Se si sposta o si importa una macchina virtuale in un server che esegue Hyper-V in Windows Server 2016 o Windows Server 2019 da Windows Server 2012 R2, la configurazione della macchina virtuale non viene aggiornata automaticamente. Ciò significa che è possibile riportare la macchina virtuale a un server che esegue Windows Server 2012 R2. Tuttavia, ciò significa anche fino a quando non si aggiorna manualmente la versione di configurazione della macchina virtuale non è possibile utilizzare le nuove funzionalità di macchina virtuale.  
  
Per ulteriori informazioni sulla verifica e l'aggiornamento della versione, vedere [versione aggiornamento macchina virtuale](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). In questo articolo sono elencati anche la versione in cui sono state introdotte alcune funzionalità.   
  
> [!IMPORTANT]  
> -   Dopo aver aggiornato la versione, è possibile spostare la macchina virtuale a un server che esegue Windows Server 2012 R2.  
> -   È possibile effettuare il downgrade la configurazione di una versione precedente.  
> -   Il [Update-VMVersion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) cmdlet è bloccata su un Cluster Hyper-V quando il livello funzionale del cluster è Windows Server 2012 R2.  

### <a name="virtualization-based-security-for-generation-2-virtual-machines-new"></a>Sicurezza Virtualization-based per le macchine virtuali di generazione 2 \(nuovi)

Protezione basata su virtualizzazione alimenta funzionalità quali protezione del dispositivo e il controllo delle credenziali che offrono una maggiore protezione del sistema operativo contro gli attacchi da malware. La protezione basata di virtualizzazione è disponibile nelle macchine virtuali di generazione 2 guest a partire dalla versione 8. Per informazioni sulla versione di macchina virtuale, vedere [versione aggiornamento macchina virtuale in Hyper-V in Windows 10 o Windows Server 2016](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md).

### <a name="windows-containers-new"></a>I contenitori Windows \(new\)

Contenitori di Windows consentono molte applicazioni isolate per l'esecuzione in un sistema di computer. Che è veloce per compilare e sono estremamente scalabili e portabile. Sono disponibili due tipi di runtime di contenitore, ciascuno con un diverso livello di isolamento delle applicazioni. Windows Server Containers utilizzare l'isolamento di processo e lo spazio dei nomi. Contenitori di Hyper-V utilizzano una macchina virtuale leggera per ogni contenitore.  
  
Funzionalità principali includono:  
  
-   Supporto per siti web e applicazioni utilizzando il protocollo HTTPS  
  
-   Nano server può ospitare sia Windows Server e i contenitori di Hyper-V  
  
-   Possibilità di gestire i dati all'interno delle cartelle condivise contenitore  
  
-   Possibilità di limitare le risorse di contenitore  
  
Per altre informazioni, incluse guide introduttive, vedere la [documentazione sui contenitori Windows](https://docs.microsoft.com/virtualization/windowscontainers/index).  
  
### <a name="windows-powershell-direct-new"></a>Windows PowerShell diretta \(new\)

Ciò consente di eseguire i comandi di Windows PowerShell in una macchina virtuale dall'host. Windows PowerShell diretta viene eseguita tra l'host e la macchina virtuale. Ciò significa non richiede la rete o i requisiti del firewall e funziona indipendentemente dalla configurazione di gestione remota.  
  
Windows PowerShell diretto è un'alternativa per gli strumenti esistenti utilizzati dagli amministratori di Hyper-V per connettersi a una macchina virtuale in un host Hyper-V:  
  
-   Strumenti di gestione remota, ad esempio PowerShell o Desktop remoto  
  
-   Hyper-V Virtual Machine Connection (VMConnect)  
  
Questi strumenti funzionano bene, ma sono compromessi: VMConnect è affidabile, ma può essere difficile da automatizzare. PowerShell remoto è potente, ma può essere difficile da configurare e gestire. Questi compromessi potrebbero diventare più importanti man mano che aumenta la distribuzione di Hyper-V. Windows PowerShell diretto risolve questo problema fornendo un'esperienza di scripting e automazione potenti che è semplice come l'uso di VMConnect.
  
Per i requisiti e le istruzioni, vedere [Windows di gestire le macchine virtuali con PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md).  
