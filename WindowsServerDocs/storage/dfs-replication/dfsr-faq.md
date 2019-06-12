---
Title: 'Replica DFS: Domande frequenti'
ms.date: 06/18/2014
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 280104977e295ce0c9ccb05b806442ccaa73667b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447235"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>Replica DFS: Domande frequenti


Aggiornamento: 30 aprile 2019

Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Queste domande frequenti sulla replica di File System distribuito (DFS) (noto anche come DFS-R o DFSR) per Windows Server.

Per informazioni su Spazi dei nomi DFS, vedere il documento di [domande frequenti su Domande frequenti](https://technet.microsoft.com/en-us/library/ee404780).

Per informazioni sulle novità in replica DFS, vedere gli argomenti seguenti:

  - [DFS Namespaces and DFS Replication Overview](http://technet.microsoft.com/en-us/library/jj127250) (in Windows Server 2012)  
      
  - [What ' s New in File System distribuito](https://technet.microsoft.com/en-us/library/ee307957) nell'argomento [modifiche apportate alle funzionalità da Windows Server 2008 a Windows Server 2008 R2](https://technet.microsoft.com/en-us/library/dd391932)  
      
  - [File System distribuito](https://technet.microsoft.com/en-us/library/cc753479) nell'argomento [modifiche apportate alle funzionalità da Windows Server 2003 con SP1 per Windows Server 2008](https://technet.microsoft.com/en-us/library/cc753208)  
      

Per ottenere un elenco delle modifiche apportate di recente a questo argomento, vedere la sezione [Cronologia delle modifiche](#change-history) di questo argomento.

      

## <a name="interoperability"></a>Interoperabilità

### <a name="can-dfs-replication-communicate-with-frs"></a>Replica DFS possono comunicare con il servizio Replica file?

No. Replica DFS non comunica con servizio Replica File (FRS). Replica DFS e il servizio Replica file possono essere eseguiti nello stesso server allo stesso tempo, ma mai devono essere configurati per replicare le stesse cartelle o sottocartelle in quanto questa operazione può provocare la perdita di dati.

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>Se replica DFS può sostituire FRS per la replica di SYSVOL

Sì, replica DFS può sostituire il servizio Replica file per la replica di SYSVOL nei server che eseguono Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. Server che eseguono Windows Server 2003 R2 non supportano l'utilizzo di replica DFS per replicare la cartella SYSVOL.

Per altre informazioni sulla replica di SYSVOL con replica DFS, vedere il [Guida alla migrazione di replica di SYSVOL: Replica da FRS a DFS](https://technet.microsoft.com/en-us/library/dd640019).

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>È possibile aggiornare da FRS a DFS senza perdere le impostazioni di configurazione?

Sì. Per eseguire la migrazione di replica da FRS a replica DFS, vedere i documenti seguenti:

  - Per eseguire la migrazione della replica delle cartelle diverse dalla cartella SYSVOL, vedere [Guida operativa di DFS: Migrazione da FRS a DFS](http://go.microsoft.com/fwlink/?linkid=192776) e [FRS2DFSR: una replica da FRS a replica DFS Migration Utility](http://go.microsoft.com/fwlink/?linkid=195437) (http://go.microsoft.com/fwlink/?LinkID=195437).  
      
  - Per eseguire la migrazione della replica della cartella SYSVOL per replica DFS, vedere [Guida alla migrazione di replica di SYSVOL: Replica da FRS a DFS](https://technet.microsoft.com/en-us/library/dd640019).  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>È possibile usare replica DFS in un ambiente misto Windows/UNIX?

Sì. Anche se replica DFS supporta solo la replica di contenuto tra server che eseguono Windows Server, i client UNIX possono accedere le condivisioni file nei server Windows. A tale scopo, installare i servizi per sistemi di File di rete (NFS) nel server di replica DFS.

È anche possibile usare la funzionalità del client SMB/CIFS inclusa in molti client UNIX di accedere direttamente condivisioni di file di Windows, anche se questa funzionalità è spesso limitata o richiede modifiche all'ambiente di Windows (ad esempio disabilitando la firma SMB usando Criteri di gruppo).

Replica DFS interagisce con NFS in un server che esegue un sistema operativo Windows Server, ma non è possibile replicare un punto di montaggio NFS.

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>È possibile usare il servizio Copia Shadow del Volume con replica DFS?

Sì. Replica DFS è supportata nei volumi del servizio Copia Shadow del Volume (VSS) e gli snapshot precedenti possono essere ripristinati correttamente con il Client di versioni precedenti.

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>È possibile usare Backup di Windows (Ntbackup.exe) di backup in remoto a una cartella replicata?

No, utilizza Windows Backup (Ntbackup.exe) in un computer che esegue Windows Server 2003 o versione precedente per eseguire il backup del contenuto di una cartella replicata in un computer che esegue Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 non è supportato.

Per eseguire il backup dei file archiviati in una cartella replicata, usare Windows Server Backup o Microsoft® System Center Data Protection Manager. Per informazioni sulle funzionalità di Backup e ripristino in Windows Server 2008 R2 e Windows Server 2008, vedere [Backup e ripristino](https://technet.microsoft.com/en-us/library/Cc754097). Per altre informazioni, vedere [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=182261) (http://go.microsoft.com/fwlink/?LinkId=182261).

### <a name="do-file-system-policies-impact-dfs-replication"></a>I criteri di file system influisce sulla replica DFS?

Sì. Non configurare i criteri di sistema di file nelle cartelle replicate. Il criterio del file system riapplica le autorizzazioni NTFS in ogni intervallo di aggiornamento di criteri di gruppo. Ciò può comportare le violazioni di condivisione perché un file aperto non viene replicato fino a quando il file viene chiuso.

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>Replica DFS replica cassette postali ospitate in Microsoft Exchange Server?

No. Replica DFS non può essere usata per replicare le cassette postali ospitate in Microsoft Exchange Server.

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>Replica DFS supporta gli screening dei file creati da Gestione risorse File Server?

Sì. Tuttavia, di screening di impostazioni di gestione risorse File Server (FSRM) devono corrispondere in entrambe le estremità della replica. Inoltre, replica DFS ha un proprio meccanismo di filtro per i file e cartelle che è possibile usare per escludere determinati file e i tipi di file dalla replica.

Di seguito sono procedure consigliate per implementare le quote o screening dei file:

  - La cartella DfsrPrivate nascosta non deve essere soggette a quote o screening dei file.  
      
  - File selezionati non devono esistere in qualsiasi cartella replicata prima indagine è abilitata.  
      
  - Nessuna cartella potrebbe superare la quota prima che la quota è abilitata.  
      
  - È necessario utilizzare le quote disco rigide con cautela. È possibile che i singoli membri di un gruppo di replica vengano mantenute all'interno di una quota prima della replica, ma, quando si deve superare i file vengono replicati. Ad esempio, se un utente copia un file di 10 megabyte (MB) nel server A (che è quindi raggiunge il limite rigido) e un altro utente copia un file di 5 MB nel server B, quando viene eseguita la replica successiva, entrambi i server supererà la quota da 5 MB. Ciò può causare la replica DFS ripetere continuamente la replica di file, può causare problemi, il vettore di versione e problemi di prestazioni possibili.  
      

### <a name="is-dfs-replication-cluster-aware"></a>È compatibile con cluster di replica DFS?

Sì, replica DFS in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2 include la possibilità di aggiungere un cluster di failover come membro di un gruppo di replica. Per altre informazioni, vedere [aggiungere un Cluster di Failover a un gruppo di replica](http://go.microsoft.com/fwlink/?linkid=155085) (http://go.microsoft.com/fwlink/?LinkId=155085). Il servizio Replica DFS su versioni di Windows prima di Windows Server 2008 R2 non è progettato per il coordinamento con un cluster di failover e il servizio non eseguirà il failover a un altro nodo.


> [!NOTE]
> Replica DFS non supporta la replica file in volumi condivisi Cluster. 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>È compatibile con la deduplicazione dei dati di replica DFS?

Sì, replica DFS possono replicare le cartelle nei volumi che usano la deduplicazione dei dati in Windows Server.

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>Replica DFS con è compatibile RIS e WDS?

Sì. La replica DFS replica i volumi in cui è abilitato Single Instance Storage (SIS). SIS viene utilizzato da servizi di installazione remota (RIS), servizi di distribuzione Windows (WDS) e Windows Storage Server.

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>È possibile usare replica DFS con i file Offline?

È possibile usare replica DFS e i file Offline contemporaneamente in scenari quando è presente un solo utente alla volta chi scrive i file. Ciò è utile per gli utenti di viaggio tra due filiali e si desidera essere in grado di accedere ai file in dei rami o quando non in linea. I file offline memorizza nella cache i file in locale per l'uso offline e replica DFS replica i dati tra ogni succursale.

Non usare replica DFS con i file Offline in un ambiente multiutente perché la replica DFS non è incluso qualsiasi meccanismo di blocco distribuita o funzionalità di estrazione di file. Se due utenti modificano lo stesso file contemporaneamente su diversi server, replica DFS sposta il file meno recente di DfsrPrivate\\cartelle ConflictandDeleted (che si trova nel percorso locale della cartella replicata) durante la replica successiva.

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>Quali applicazioni antivirus sono compatibili con replica DFS?

Le applicazioni antivirus possono causare eccessiva replica se le attività di scansione di modificare i file in una cartella replicata. Per altre informazioni, [Testing l'interoperabilità delle applicazioni Antivirus con replica DFS](http://go.microsoft.com/fwlink/?linkid=73990) (http://go.microsoft.com/fwlink/?LinkId=73990).

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>Quali sono i vantaggi dell'uso della replica DFS invece di Windows SharePoint Services?

Windows® SharePoint® Services offre una stretta coerenza sotto forma di funzionalità di estrazione di file che replica DFS non sono presenti. Se si è preoccupati per più utenti che stanno modificando lo stesso file, è consigliabile usare Windows SharePoint Services. Windows SharePoint Services 2.0 con Service Pack 2 è disponibile come parte di Windows Server 2003 R2. Windows SharePoint Services può essere scaricato dal sito Web Microsoft; non è incluso nelle versioni più recenti di Windows Server. Tuttavia, se si replicano i dati in più siti e gli utenti non modificherà gli stessi file nello stesso momento, replica DFS offre maggiore larghezza di banda e la gestione più semplice.

## <a name="limitations-and-requirements"></a>Requisiti e limitazioni

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>Replica DFS è possibile replicare tra succursali senza una connessione VPN?

Sì, presupponendo che sia disponibile un collegamento di Wide Area Network (WAN) privato, non Internet, la connessione delle succursali. Tuttavia, è necessario aprire le porte appropriate nei firewall esterno. Replica DFS Usa l'agente mapping Endpoint RPC (porta 135) e una porta assegnata casualmente temporanea superiore a 1024. È possibile usare la **Dfsrdiag** strumento da riga di comando per specificare una porta statica invece le porte temporanee. Per altre informazioni su come specificare l'agente mapping Endpoint RPC, vedere [articolo 154596](http://go.microsoft.com/fwlink/?linkid=73991) della Microsoft Knowledge Base (http://go.microsoft.com/fwlink/?LinkId=73991).

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>Replica DFS possono replicare i file crittografati con la crittografia del File System?

No. Replica DFS non vengono replicate file o cartelle che vengono crittografate con la crittografia File System (EFS). Se si crittografa un file che sono stato replicato in precedenza, replica DFS Elimina il file da tutti gli altri membri del gruppo di replica. Ciò garantisce che la copia è disponibile solo del file è la versione crittografata nel server.

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>Replica DFS possono replicare i file di database di Microsoft Office Access o pst di Outlook?

Replica DFS possono replicare in modo sicuro Microsoft Outlook cartelle personali (pst) e i file di Access solo se sono archiviati per scopi di archiviazione e non sono accessibili attraverso la rete usando un client come Outlook o accesso (per aprire file pst o accesso i file, copiare innanzitutto i file in un dispositivo di archiviazione locale). I motivi sono i seguenti:

  - Apertura di file. pst su connessioni di rete potrebbe causare il danneggiamento dei dati nei file. pst. Per altre informazioni sul perché i file. pst non è possibile l'accesso sicuro da attraverso una rete, vedere [articolo 297019](http://go.microsoft.com/fwlink/?linkid=125363) della Microsoft Knowledge Base (http://go.microsoft.com/fwlink/?LinkId=125363).  
      
  - i tipi di accedere ai file tendono a rimanere aperta per lunghi periodi di tempo, a cui si accede da un client come Outlook o Office Access. Ciò impedisce che la replica di questi file fino a quando non sono chiuse replica DFS.  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>È possibile utilizzare Replica DFS in un gruppo di lavoro?

No. Replica DFS si basa su Active Directory® Domain Services per la configurazione. Funzionerà solo in un dominio.

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>Più di una cartella può essere replicata in un singolo server?

Sì. Replica DFS possono replicare numerose cartelle tra server. Assicurarsi che ogni cartella replicata abbia un valore univoco radice percorso e che non si sovrappongano. Ad esempio, d:\\Sales e d:\\Accounting può essere i percorsi della radice per le cartelle replicate di due, ma l'unità d:\\vendite e d:\\Sales\\report non possono essere il percorso radice per due cartelle replicate.

### <a name="does-dfs-replication-require-dfs-namespaces"></a>Replica DFS richiede spazi dei nomi DFS?

No. Replica DFS e spazi dei nomi DFS può essere utilizzata insieme o separatamente. Inoltre, replica DFS consente di replicare gli spazi dei nomi DFS autonomo, che non era possibile con FRS.

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>Replica DFS richiede la sincronizzazione dell'ora tra i server?

No. Replica DFS non richiede esplicitamente la sincronizzazione dell'ora tra i server. Tuttavia, replica DFS richiede che gli orologi del server corrispondano da vicino. Gli orologi del server devono essere impostati entro cinque minuti uno da altro (per impostazione predefinita) per l'autenticazione Kerberos funzioni correttamente. Ad esempio, replica DFS utilizza timestamp per determinare quali file ha la precedenza in caso di conflitto. Accurate tempi sono importanti anche per operazioni di garbage collection, pianificazioni e altre funzionalità.

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>Replica DFS supporta la replica di un intero volume?

Sì. Tuttavia, è necessario prima installare Windows Server 2003 Service Pack 2 o l'hotfix. Per altre informazioni, vedere [articolo 920335](http://go.microsoft.com/fwlink/?linkid=76776) della Microsoft Knowledge Base (http://go.microsoft.com/fwlink/?LinkId=76776). Inoltre, la replica di un intero volume può causare i problemi seguenti:

  - Se il volume contiene un file di paging di Windows, la replica ha esito negativo e Registra evento di replica DFS 4312 nel registro eventi di sistema.  
      
  - Replica DFS imposta gli attributi di sistema e nascosto nella cartella replicata nei server di destinazione. Ciò si verifica perché Windows applica gli attributi di sistema e nascosto per la cartella radice del volume per impostazione predefinita. Se il percorso locale della cartella replicata nei server di destinazione è anche una radice del volume, ulteriori modifiche non vengono apportate agli attributi della cartella.  
      
  - Quando la replica di un volume che contiene la cartella di sistema di Windows, replica DFS riconosce la cartella % WINDIR % e non viene replicato. Tuttavia, replica DFS di replicare le cartelle usate dalle applicazioni non Microsoft, che potrebbero causare il applicazioni vengono eseguite nei server di destinazione se l'applicazione presenta problemi di interoperabilità con replica DFS.  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>Replica DFS supporta RPC su HTTP?

No.

### <a name="does-dfs-replication-work-across-wireless-networks"></a>Replica DFS funziona tra le reti wireless?

Sì. Replica DFS è indipendente dal tipo di connessione.

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>Replica DFS funziona in ReFS o volumi FAT?

No. Replica DFS supporta volumi formattati con il file system NTFS di sola lettura. Resilient File System (ReFS) e il file system FAT non sono supportati. Replica DFS richiede NTFS poiché utilizza il journal delle modifiche NTFS e altre funzionalità di NTFS file system.

### <a name="does-dfs-replication-work-with-sparse-files"></a>Replica DFS funziona con i file sparse?

Sì. È possibile replicare i file sparse. Il **Sparse** attributo viene mantenuto nel membro di destinazione.

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>È necessario accedere come amministratore per replicare i file?

No. Replica DFS è un servizio che viene eseguito con l'account sistema locale, pertanto non è necessario accedere come amministratore per la replica. Tuttavia, è necessario essere un amministratore di dominio o un amministratore locale del server di file interessato per apportare modifiche alla configurazione della replica DFS.

Per altre informazioni, vedere "requisiti di sicurezza della replica DFS e la delega" nel [delegare il permesso di gestire Replica DFS](http://go.microsoft.com/fwlink/?linkid=182294) (http://go.microsoft.com/fwlink/?LinkId=182294).

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>Come è possibile aggiornare o sostituire un membro di replica DFS?

Per aggiornare o sostituire un membro di replica DFS, vedere questo blog post su chiave Ask blog del Team di servizi di Directory: [Sostituzione di Hardware e membro DFSR OS](http://blogs.technet.com/b/askds/archive/2010/09/10/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os.aspx).

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>È adatto per la replica di profili mobili replica DFS?

Sì. Alcuni scenari sono supportati quando si replicano i profili utente mobili. Per informazioni sugli scenari supportati, vedere [supporto istruzione intorno a replica dati di Microsoft del profilo utente](http://go.microsoft.com/fwlink/?linkid=201282) (http://go.microsoft.com/fwlink/?LinkId=201282).

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>È disponibile un limite di caratteri file o alla profondità della cartella?

Windows e replica DFS supportano i percorsi delle cartelle con fino a migliaia di 32 caratteri. Replica DFS non è limitata per i percorsi delle cartelle di 260 caratteri.

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>I membri di un gruppo di replica devono trovarsi nello stesso dominio?

No. I gruppi di replica possono estendersi tra più domini all'interno di una singola foresta, ma non tra foreste diverse.

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>Quali sono i limiti supportati da di replica DFS?

Nell'elenco seguente offre un set di linee guida per la scalabilità che sono stati testati da Microsoft in Windows Server 2012 R2:

  - Dimensioni di tutti i file replicati in un server: 100 terabyte.  
      
  - Numero di file replicati in un volume: milioni di 70.  
      
  - Dimensioni massime del file: 250 GB.  
      


> [!IMPORTANT]
> Durante la creazione di gruppi di replica con un numero elevato o dimensioni dei file è consigliabile esportare un clone del database e usando tecniche di pre-seeding per ridurre al minimo la durata della replica iniziale. Per altre informazioni, vedere <A href="http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx">sincronizzazione iniziale di replica DFS in Windows Server 2012 R2: Attacco dei cloni</A>. 
<br>


Nell'elenco seguente offre un set di linee guida per la scalabilità che sono stati testati da Microsoft in Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008:

  - Dimensioni di tutti i file replicati in un server: 10 TB.  
      
  - Numero di file replicati in un volume: milioni di 11.  
      
  - Dimensioni massime del file: 64 GB.  
      


> [!NOTE]
> Non è più un limite al numero di gruppi di replica, le cartelle replicate, le connessioni o membri del gruppo di replica. 
<br>


Per un elenco delle linee guida per la scalabilità che sono stati testati da Microsoft per Windows Server 2003 R2, vedere [linee guida per la scalabilità di replica DFS](http://go.microsoft.com/fwlink/?linkid=75043) (http://go.microsoft.com/fwlink/?LinkId=75043).

### <a name="when-should-i-not-use-dfs-replication"></a>Quando non usare replica DFS?

Non usare replica DFS in un ambiente in cui più utenti aggiornano o modificare gli stessi file contemporaneamente in server diversi. Questa operazione può causare la replica DFS spostare in conflitto copie dei file per il DfsrPrivate nascosto\\cartelle ConflictandDeleted.

Quando più utenti devono modificare gli stessi file nello stesso momento in server diversi, usare la funzionalità di estrazione di file di Windows SharePoint Services per garantire che solo un utente sta lavorando a un file. Windows SharePoint Services 2.0 con Service Pack 2 è disponibile come parte di Windows Server 2003 R2. Windows SharePoint Services può essere scaricato dal sito Web Microsoft; non è incluso nelle versioni più recenti di Windows Server.

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>Perché è un aggiornamento dello schema necessario per la replica DFS?

Replica DFS utilizza i nuovi oggetti nel contesto dei nomi di dominio di Active Directory Domain Services per archiviare le informazioni di configurazione. Questi oggetti vengono creati quando si aggiorna lo schema di Active Directory Domain Services. Per altre informazioni, vedere [esaminare i requisiti per la replica DFS](http://go.microsoft.com/fwlink/?linkid=182264) (http://go.microsoft.com/fwlink/?LinkId=182264).

## <a name="monitoring-and-management-tools"></a>Strumenti di gestione e monitoraggio

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>È possibile automatizzare il report sull'integrità per ricevere gli avvisi?

Sì. Esistono tre modi per automatizzare i report sull'integrità:

  - Usare il modulo PowerShell di Windows di replica DFS incluso in Windows Server 2012 R2 o DfsrAdmin.exe insieme a operazioni pianificate regolarmente generare i report sull'integrità. Per altre informazioni, vedere [automatizzazione di report sull'integrità della replica DFS](http://go.microsoft.com/fwlink/?linkid=74010) (http://go.microsoft.com/fwlink/?LinkId=74010).  
      
  - Usare il Management Pack della replica DFS per System Center Operations Manager per creare avvisi basati su condizioni specificate.  
      
  - Usare il provider WMI di replica DFS per gli avvisi di script.  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>È utilizzare Microsoft System Center Operations Manager per monitorare la replica DFS?

Sì. Per altre informazioni, vedere la [DFS Replication Management Pack per System Center Operations Manager 2007](http://go.microsoft.com/fwlink/?linkid=182265) in Microsoft Download Center (http://go.microsoft.com/fwlink/?LinkId=182265).

### <a name="does-dfs-replication-support-remote-management"></a>Replica DFS supporta la gestione remota?

Sì. Replica DFS supporta la gestione remota tramite la console di gestione DFS e la **Aggiungi gruppo di replica** comando. Nel server A, ad esempio, è possibile connettersi a un gruppo di replica definito nella foresta con server A e B come membri.

Gestione DFS è incluso in Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 e Windows Server 2003 R2. Per gestire la replica DFS da altre versioni di Windows, usare Desktop remoto o la [remoto Server Administration Tools per Windows 7](https://technet.microsoft.com/en-us/library/Ee449475).


> [!IMPORTANT]
> Per visualizzare o gestire i gruppi di replica contenenti cartelle replicate di sola lettura o membri del cluster di failover, è necessario usare la versione di gestione DFS inclusa in Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, il <a href="http://go.microsoft.com/fwlink/p/?linkid=238560">Strumenti di amministrazione remota del Server per Windows 8</a>, o il <a href="https://technet.microsoft.com/en-us/library/ee449475">strumenti di amministrazione remota del Server per Windows 7</a>. 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>Ultrasound e Sonar funzionano con replica DFS?

No. Replica DFS ha un proprio set di strumenti di diagnostica e monitoraggio. Ultrasound e Sonar solo sono in grado di monitorare FRS.

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>Come è possibile ripristinare file dalle cartelle ConflictAndDeleted o PreExisting?

Ripristinare file persi, ripristinare i file dalla cartella del file system o una cartella condivisa usando cronologia File, il **Ripristina versioni precedenti** comando in Esplora File oppure ripristinare i file dal backup. Per ripristinare i file direttamente dalla cartella ConflictAndDeleted o PreExisting, usare il `Get-DfsrPreservedFiles` e `Restore-DfsrPreservedFiles` (inclusi con il modulo replica DFS in Windows Server 2012 R2), i cmdlet di Windows PowerShell o il [RestoreDFSR](http://code.msdn.microsoft.com/restoredfsr) script di esempio da MSDN Code Gallery. Questo script è disponibile solo per il ripristino di emergenza e viene fornito AS-è, senza garanzie.

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>È possibile conoscere lo stato della replica?

Sì. Esistono diversi modi per monitorare la replica:

  - Replica DFS presenta un management pack per System Center Operations Manager che fornisce il monitoraggio proattivo.  
      
  - Gestione DFS è un report di diagnostica in arrivo per il backlog di replica, l'efficienza della replica e il numero di file e cartelle in un gruppo di replica specificato.  
      
  - Il modulo PowerShell di Windows replica DFS in Windows Server 2012 R2 contiene i cmdlet per avviare i test di propagazione e la creazione di report di propagazione e l'integrità. Per altre informazioni, vedere [distribuiti i cmdlet di replica di File System in Windows PowerShell](http://technet.microsoft.com/library/dn296601.aspx).  
      
  - Dfsrdiag.exe è uno strumento da riga di comando che può generare un conteggio di backlog o un trigger di un test di propagazione. Entrambe mostrano lo stato della replica. Propagazione descrive se i file vengono replicati in tutti i nodi. Backlog Mostra il numero di file è comunque necessario eseguire la replica prima di due computer siano sincronizzati. Il conteggio di backlog è il numero di aggiornamenti che un membro del gruppo di replica non è elaborata. Nei computer che eseguono Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2, Dfsrdiag.exe può anche visualizzare gli aggiornamenti che è attualmente la replica di replica DFS.  
      
  - Gli script possono utilizzare WMI per raccogliere le informazioni di backlog, manualmente o tramite MOM.  
      

## <a name="performance"></a>Prestazioni

### <a name="does-dfs-replication-support-dial-up-connections"></a>Replica DFS supporta le connessioni remote?

Anche se replica DFS funziona a velocità di connessione remota, è possibile ottenere nel backlog se sono presenti un numero elevato di replica delle modifiche. Se vengono apportate piccole modifiche ai file esistenti, replica DFS con la compressione RDC (Remote Differential) fornisce prestazioni molto elevate rispetto alla copia il file direttamente.

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>Replica DFS esegue il rilevamento della larghezza di banda?

No. Replica DFS non esegue il rilevamento della larghezza di banda. È possibile configurare la replica DFS per usare una quantità limitata di larghezza di banda in una singola connessione (limitazione della larghezza di banda). Tuttavia, replica DFS non ridurre ulteriormente Utilizzo larghezza di banda se l'interfaccia di rete diventa saturo e replica DFS possono saturare il collegamento per brevi periodi. Larghezza di banda di limitazione delle richieste con replica DFS non è perfettamente accurata perché la replica DFS Limita larghezza di banda grazie alla limitazione delle chiamate RPC. Di conseguenza, può interferire diversi buffer nei livelli inferiori dello stack di rete (incluso il RPC), causando i picchi di traffico di rete.

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>Replica DFS limitare la larghezza di banda per ogni pianificazione, per ogni server o per ogni connessione?

Se si configura la limitazione della larghezza di banda quando si specifica la pianificazione, tutte le connessioni per tale gruppo di replica utilizzerà tale impostazione per la limitazione della larghezza di banda. La limitazione della larghezza di banda può anche essere impostata come un'impostazione a livello di connessione mediante Gestione DFS.

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>Replica DFS Usa Active Directory Domain Services per calcolare i collegamenti di sito e i costi di connessione?

No. Replica DFS utilizza la topologia definita dall'amministratore, che è indipendente dal calcolo dei costi del sito di Active Directory Domain Services.

### <a name="how-can-i-improve-replication-performance"></a>Come è possibile migliorare le prestazioni della replica?

Per altre informazioni sui vari metodi di ottimizzazione delle prestazioni della replica, vedere [ottimizzazione delle prestazioni di replica in replica DFS](http://blogs.technet.com/b/askds/archive/2010/03/31/tuning-replication-performance-in-dfsr-especially-on-win2008-r2.aspx) nel [chiedere il blog del Team di servizi di Directory](http://blogs.technet.com/b/askds/).

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>Come replica DFS evitare la saturazione una connessione?

La replica DFS è impostare la larghezza di banda massima da usare in una connessione e il servizio gestisce tale livello di utilizzo della rete. Questo comportamento è diverso dal servizio trasferimento intelligente in Background (BITS) e replica DFS non saturi la connessione se si imposta in modo appropriato.

Ciononostante, la limitazione della larghezza di banda non è accurato al 100% e replica DFS possono saturare il collegamento per brevi periodi di tempo. Questo avviene perché replica DFS Limita larghezza di banda grazie alla limitazione delle chiamate RPC. Poiché questo processo si basa su diversi buffer nei livelli inferiori dello stack di rete, incluso il RPC, il traffico di replica tende a viaggiare in picchi che in alcuni casi possono saturare i collegamenti di rete.

Replica DFS in Windows Server 2008 include numerosi miglioramenti delle prestazioni, come descritto nella [File System distribuito](https://technet.microsoft.com/en-us/library/Cc753479), un argomento in [modifiche apportate alle funzionalità da Windows Server 2003 con SP1 per Windows Server 2008](https://technet.microsoft.com/en-us/library/cc753208).

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>Quali differenze tra le prestazioni della replica DFS con il servizio Replica file?

Replica DFS è molto più veloce rispetto a FRS, in particolare quando vengono apportate piccole modifiche al file di grandi dimensioni e la tecnologia RDC è abilitata. Ad esempio, con connessione desktop remoto, una piccola modifica a una presentazione MB PowerPoint® 2 può comportare solo 60 kilobyte (KB inviati attraverso la rete), ovvero un risparmio del 97% di byte trasferiti.

Connessione desktop remoto non viene utilizzato nel file di dimensioni inferiori a 64 KB e potrebbe non essere utile in reti LAN ad alta velocità in cui non è conflitti della larghezza di banda di rete. È possibile disabilitare la tecnologia RDC su una base per ogni connessione mediante Gestione DFS.

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>Frequenza con cui la replica DFS replica i dati?

I dati vengono replicati con la pianificazione che è impostato. Ad esempio, è possibile impostare la pianificazione a intervalli di 15 minuti, sette giorni alla settimana. Durante tali intervalli, la replica è abilitata. La replica viene avviata subito dopo viene rilevata una modifica di file (in genere entro pochi secondi).

La pianificazione del gruppo di replica può essere impostato per Universal Time Coordinate (UTC) durante la pianificazione di connessione è impostata sull'ora locale del membro di destinazione. Tenerne conto quando il gruppo di replica si estende su più fusi orari. Ora locale si intende il tempo del membro che ospita la connessione in ingresso. La pianificazione visualizzata della connessione in ingresso e la connessione in uscita corrispondente riflettono le differenze di fuso orario durante la pianificazione è impostata nell'ora locale.

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>Quantità di risorse di sistema del mio server utilizzerà la replica DFS?

Il disco, memoria e risorse della CPU utilizzate dalla replica DFS dipendono da diversi fattori, tra cui il numero e le dimensioni del file, frequenza di modifica, numero di membri del gruppo di replica e numero di cartelle replicate. Inoltre, alcune risorse sono più difficili da stimare. Ad esempio, la tecnologia di archiviazione motore ESE (Extensible) usata per il database di replica DFS può utilizzare un'alta percentuale di memoria disponibile, che rilascia su richiesta. Applicazioni diverse da replica DFS possono essere ospitate nello stesso server in base alla configurazione del server. Tuttavia, quando si ospitano più applicazioni o ruoli del server in un singolo server, è importante testare questa configurazione prima di implementarlo in un ambiente di produzione.

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>Cosa accade se si verifica un errore durante la replica di un collegamento WAN.

Se la connessione diventa inattivo, replica DFS è proverà a replicare mentre la pianificazione è aperta. Esisterà inoltre gli errori di connettività indicati nel registro eventi di replica DFS che possono essere raccolti utilizzando MOM (in modo proattivo con avvisi) e il rapporto di stato di replica DFS (in modo reattivo, ad esempio quando un amministratore lo esegue).

## <a name="remote-differential-compression-details"></a>Dettagli di compressione differenziale remoti

### <a name="are-changes-compressed-before-being-replicated"></a>Le modifiche vengono compressi prima di essere replicati?

Sì. Le parti modificate di file vengono compressi prima di essere inviati per tutti i tipi eccetto il seguente (che sono già compressi) di file:. wma,. wmv, con estensione zip,. jpg,. mpg,. MPEG, m1v, mp2, MP3, MPA, con estensione cab, wav,. snd, au,. asf,. wm,. avi, dell'alfabeto latino, GZ, con estensione TGZ e frx. Le impostazioni di compressione per questi tipi di file non sono configurabili in Windows Server 2003 R2.

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>Un amministratore può disattivare la RDC o modificare la soglia?

Sì. È possibile disattivare la tecnologia RDC tramite la pagina delle proprietà di una determinata connessione. La disabilitazione di RDC può ridurre la latenza del CPU replica e di utilizzo su collegamenti di rete veloce locale (LAN) che non dispongono di alcun vincolo di larghezza di banda o per gruppi di replica principalmente costituiti da file di dimensioni inferiori a 64 KB. Se si sceglie di disabilitare la connessione desktop remoto su una connessione, l'efficienza della replica di test prima e dopo la modifica per verificare che si hanno migliorato le prestazioni della replica.

È possibile modificare la soglia delle dimensioni di connessione desktop remoto usando il **Dfsradmin connessione impostata** comando, il Provider WMI di replica DFS o modificando manualmente il file XML di configurazione del file.

### <a name="does-rdc-work-on-all-file-types"></a>RDC funziona in tutti i tipi di file?

Sì. RDC calcola le differenze a livello di blocco indipendentemente dal tipo di dati di file. Tuttavia, la tecnologia RDC funziona in modo più efficiente su determinati tipi di file, ad esempio documenti di Word, file PST e immagini disco rigido virtuale.

### <a name="how-does-rdc-work-on-a-compressed-file"></a>Come funziona la tecnologia RDC in un file compresso?

Replica DFS Usa RDC, che calcola i blocchi nel file che sono stati modificati e invia solo i blocchi sulla rete. Replica DFS non è necessario conoscere il contenuto del file, ovvero solo quali blocchi sono stati modificati.

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>È compressione RDC abilitata durante l'aggiornamento a Windows Server Enterprise Edition o Datacenter Edition?

Windows Server Standard Edition non supportano compressione RDC tra file. Tuttavia, viene abilitato automaticamente quando esegue l'aggiornamento a un'edizione che supporta la compressione RDC o se un membro della connessione di replica è in esecuzione un'edizione supportata. Per un elenco di edizioni che supporto compressione i RDC tra file, vedere edizioni di sistemi operativi supportati Windows compressione RDC tra file?

### <a name="is-rdc-true-block-level-replication"></a>È la tecnologia RDC true per la replica a livello di blocco?

No. La tecnologia RDC è un protocollo generico per la compressione di trasferimento di file. Replica DFS Usa RDC relativa ai blocchi a livello di file, non a livello di blocco del disco. RDC suddivide il file in blocchi. Per ogni blocco in un file, consente di calcolare una firma, che è un numero ridotto di byte che può rappresentare il blocco di dimensioni maggiori. Il set di firme vengono trasferiti dal server al client. Il client confronta le firme di server per il proprio. Il client quindi le richieste server di inviare solo i dati per le firme non sono già nel client.

### <a name="what-happens-if-i-rename-a-file"></a>Che cosa accade se si rinomina un file?

Replica DFS Rinomina il file su tutti gli altri membri del gruppo di replica durante la replica successiva. I file vengono rilevate mediante un ID univoco, pertanto si rinomina un file e lo spostamento che di file all'interno della replica non ha alcun effetto sulla capacità di replica DFS per replicare un file.

### <a name="what-is-cross-file-rdc"></a>Che cos'è compressione RDC tra file?

Compressione RDC consente a replica DFS Usa RDC anche quando un file con lo stesso nome non esiste sul lato client. Compressione RDC utilizza un'euristica per determinare i file che sono simili al file che deve essere replicata e Usa i blocchi dei file simili che sono identici al file di replica per ridurre al minimo la quantità di dati trasferiti sulla WAN. Compressione RDC possono usare i blocchi di fino a cinque file simili in questo processo.

Usare compressione RDC tra file, un membro della connessione di replica deve essere in esecuzione un'edizione di Windows che supporta la compressione i RDC tra file. Per un elenco di edizioni che supporto compressione i RDC tra file, vedere edizioni di sistemi operativi supportati Windows compressione RDC tra file?

### <a name="what-is-rdc"></a>Che cos'è la tecnologia RDC?

Compressione differenziale remota (RDC) è un protocollo client-server che può essere utilizzato per aggiornare in modo efficiente i file in una rete di larghezza di banda limitata. RDC rileva gli inserimenti, rimozioni e ridisposizioni di dati nei file, l'abilitazione della replica DFS di replicare solo le modifiche quando vengono aggiornati i file. La tecnologia RDC è utilizzato solo per i file che sono di 64 KB o superiori per impostazione predefinita. La tecnologia RDC è possibile usare una versione precedente di un file con lo stesso nome nella cartella replicata o nella finestra di DfsrPrivate\\cartelle ConflictandDeleted (che si trova nel percorso locale della cartella replicata).

### <a name="when-is-rdc-used-for-replication"></a>Quando si usa la tecnologia RDC per la replica?

Connessione desktop remoto viene usato quando il file supera una soglia di dimensioni minime. Questa soglia delle dimensioni è 64 KB per impostazione predefinita. Dopo aver replicato un file che superano tale soglia, le versioni aggiornate del file sempre usano connessione desktop remoto, a meno che gran parte del file viene modificata RDCE è disabilitato.

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>Quali edizioni di sistemi operativi supportati Windows compressione RDC tra file?

Usare compressione RDC tra file, un membro della connessione di replica deve essere in esecuzione un'edizione del sistema operativo Windows che supporta la compressione i RDC tra file. La tabella seguente illustra le edizioni del sistema operativo Windows supportano compressione RDC tra file.

### <a name="cross-file-rdc-availability-in-editions-of-the-windows-operating-system"></a>Compressione disponibilità RDC nelle edizioni del sistema operativo Windows

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Versione del sistema operativo</th>
<th>Standard Edition</th>
<th>Enterprise Edition</th>
<th>Datacenter Edition</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>Windows Server 2012 R2</p></td>
<td><p>Sì<em></p></td>
<td><p>Non disponibile</p></td>
<td><p>Yes</em></p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012</p></td>
<td><p>Yes</p></td>
<td><p>Non disponibile</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2008 R2</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2008</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2003 R2</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>No</p></td>
</tr>
</tbody>
</table>

\* È possibile disabilitare tra file di connessione desktop remoto in Windows Server 2012 R2.

## <a name="replication-details"></a>Dettagli della replica

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>È possibile modificare il percorso per una cartella replicata dopo averla creata?

No. Se è necessario modificare il percorso di una cartella replicata, è necessario eliminarlo in Gestione DFS e aggiungerlo nuovamente come una nuova cartella replicata. Replica DFS Usa quindi la compressione differenziale remota (RDC) per eseguire una sincronizzazione che determina se i dati sono lo stesso per l'invio e ricezione di membri. Non viene replicato tutti i dati nella cartella nuovamente.

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>È possibile configurare quali attributi di file vengono replicati?

No, non è possibile configurare quali attributi di file che replica DFS viene replicato.

Per un elenco di valori di attributo e le relative descrizioni, vedere [attributi di File](http://go.microsoft.com/fwlink/?linkid=182268) in MSDN (http://go.microsoft.com/fwlink/?LinkId=182268).

I seguenti valori di attributo possono essere impostati le `SetFileAttributes dwFileAttributes` (funzione) essi vengono replicati tramite replica DFS. Le modifiche apportate a questi valori di attributo avvia la replica degli attributi. Il contenuto del file non viene replicato, a meno che il contenuto viene modificato anche. Per altre informazioni, vedere [funzione SetFileAttributes](http://go.microsoft.com/fwlink/?linkid=182269) in MSDN library (http://go.microsoft.com/fwlink/?LinkId=182269).

  - FILE\_ATTRIBUTO\_NASCOSTI  
      
  - FILE\_ATTRIBUTO\_READONLY  
      
  - FILE\_ATTRIBUTO\_SISTEMA  
      
  - FILE\_ATTRIBUTO\_NON\_CONTENUTO\_INDICIZZATO  
      
  - FILE\_ATTRIBUTE\_OFFLINE  
      

I seguenti valori di attributo vengono replicati tramite replica DFS, ma non vengono attivati la replica.

  - FILE\_ATTRIBUTO\_ARCHIVE  
      
  - FILE\_ATTRIBUTO\_NORMALE  
      

I valori di attributo di file seguenti attivano anche la replica, anche se non possono essere impostate usando il `SetFileAttributes` funzione (usare il `GetFileAttributes` funzione per visualizzare i valori di attributo).

  - FILE\_ATTRIBUTO\_REPARSE\_PUNTO  
      

> [!NOTE]
> Replica DFS non replica i valori di attributo punto reparse point, a meno che il tag di reparse point è IO_REPARSE_TAG_SYMLINK. File con i tag di reparse point IO_REPARSE_TAG_DEDUP, IO_REPARSE_TAG_SIS o IO_REPARSE_TAG_HSM vengono replicati come file normali. Tuttavia, il tag di reparse point e i buffer di dati di reparse point non vengono replicati in altri server perché il punto di analisi può essere eseguita solo nel sistema locale. 
<br>

  - FILE\_ATTRIBUTO\_COMPRESSED  
      
  - FILE\_ATTRIBUTO\_CRITTOGRAFATO  
      

> [!NOTE]
> Replica DFS non replica i file che vengono crittografati usando la crittografia File System (EFS). Replica DFS di replicare i file che vengono crittografati usando software non Microsoft, ma solo se non imposta il valore dell'attributo FILE_ATTRIBUTE_ENCRYPTED sul file. 
<br>

  - FILE\_ATTRIBUTE\_SPARSE\_FILE  
      
  - FILE\_ATTRIBUTO\_DIRECTORY  
      

Replica DFS non replica FILE\_ATTRIBUTO\_valore TEMPORANEO.

### <a name="can-i-control-which-member-is-replicated"></a>È possibile controllare quale membro viene replicato?

Sì. Quando si crea un gruppo di replica, è possibile scegliere una topologia. Oppure è possibile selezionare **Nessuna topologia** e configurare manualmente le connessioni dopo avere creato il gruppo di replica.

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>È possibile inizializzare un membro del gruppo di replica con i dati prima della replica iniziale?

Sì. Replica DFS supporta la copia dei file a un membro del gruppo di replica prima della replica iniziale. Questo "pre-installazione" può ridurre notevolmente la quantità di dati replicati durante la replica iniziale.

La replica iniziale non è necessario eseguire la replica di contenuto quando i file sono diversi solo per attributi reali o timestamp. Un attributo reale è un attributo che può essere impostato tramite la funzione Win32 `SetFileAttributes`. Per altre informazioni, vedere [funzione SetFileAttributes](http://go.microsoft.com/fwlink/?linkid=182269) in MSDN library (http://go.microsoft.com/fwlink/?LinkId=182269). Se due file sono diversi dagli altri attributi, ad esempio la compressione, i contenuti del file vengono replicati.

Per pre-installare un membro del gruppo di replica, copiare i file nella cartella appropriata nei server di destinazione, creare il gruppo di replica e quindi scegliere un membro primario. Scegliere il membro contenente i file più aggiornati che si vuole replicare in quanto il contenuto del membro primario viene considerato "attendibile". Ciò significa che durante la replica iniziale, i file del membro primario verranno sovrascritto le altre versioni dei file in altri membri del gruppo di replica.

Per informazioni sulla pre-seeding e la clonazione del database di replica DFS, vedere [sincronizzazione iniziale di replica DFS in Windows Server 2012 R2: Attacco dei cloni](http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx).

Per altre informazioni sulla replica iniziale, vedere [creare un gruppo di replica](https://technet.microsoft.com/en-us/library/cc725893).

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>Replica DFS superare i problemi comuni di File Replication Service?

Sì. Replica DFS consente di superare tre problemi FRS comuni:

  - Esegue il wrapping del giornale di registrazione: Replica DFS consente di recuperare da esegue il wrapping del giornale di registrazione in tempo reale. Ogni file o una cartella verrà contrassegnato come journalWrap e verificato rispetto al file system prima che la replica viene abilitata di nuovo. Durante il ripristino, questo volume non è disponibile per la replica in entrambe le direzioni.  
      
  - Replica di un numero eccessivo: Per evitare che la replica eccessiva, replica DFS Usa un sistema di riconoscimenti.  
      
  - U e v cartelle: Per evitare che i nomi delle cartelle u e v, replica DFS archivia i dati in conflitto in un DfsrPrivate nascosto\\cartelle ConflictandDeleted (che si trova nel percorso locale della cartella replicata). Ad esempio, la creazione di più cartelle contemporaneamente con nomi identici in server diversi replicati usando il servizio Replica file causa FRS rinominare le cartelle precedenti. Replica DFS Sposta invece le cartelle precedenti alla cartella Conflict and Deleted locale.  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>Replica DFS replica i file in ordine cronologico?

No. I file possono essere replicati ordinati.

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>Replica DFS replica i file che vengono utilizzati da un'altra applicazione?

Se un'applicazione apre un file e crea un blocco di file su di esso (ed evitare che ne vengano utilizzati da altre applicazioni mentre è aperto), replica DFS non vengono replicate il file fino a quando non viene chiuso. Se l'applicazione si apre il file con accesso in lettura-share, i file può ancora essere replicato.

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>Replica DFS replica le autorizzazioni di file NTFS, flussi di dati alternativi, i collegamenti reali e reparse point?

  - Replica DFS replica le autorizzazioni di file system NTFS e flussi di dati alternativi.  
      
  - Microsoft non supporta creazione i collegamenti reali di NTFS da o verso i file in una cartella replicata: questa operazione può provocare problemi di replica con i file interessati. File di collegamento vengono ignorati da replica DFS e non vengono replicati. I punti di giunzione non vengono inoltre replicati e replica DFS Registra evento 4406 per ogni punto di giunzione che rileva.  
      
  - I solo i reparse point replicati tramite replica DFS sono quelli che utilizzano il / o\_REPARSE\_TAG\_tag di collegamento SIMBOLICO; tuttavia, replica DFS non garantisce che la destinazione di un collegamento simbolico viene anche replicata. Per altre informazioni, vedere la [porre il blog del Team di servizi di Directory](http://blogs.technet.com/b/askds/archive/2011/09/30/friday-mail-sack-super-slo-mo-edition.aspx).  
      
  - I file con il / o\_REPARSE\_TAG\_DEDUP, IO\_REPARSE\_TAG\_SIS o i/o\_REPARSE\_TAG\_vengono replicati i tag di reparse point di modulo di protezione hardware come file normali. Il tag di reparse point e i buffer di dati di reparse point non vengono replicati in altri server perché il punto di analisi può essere eseguita solo nel sistema locale. Di conseguenza, replica DFS possono replicare le cartelle nei volumi che usano la deduplicazione dei dati in Windows Server 2012 o Single Instance Storage (SIS), tuttavia, le informazioni di deduplicazione dati viene gestite separatamente da ogni server in cui il servizio ruolo è abilitato.  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>Replica DFS replica le modifiche di timestamp se non altro vengono apportate modifiche al file?

No, replica DFS non replica file per cui l'unica modifica è una modifica al timestamp. Inoltre, il timestamp modificato non viene replicato ad altri membri del gruppo di replica a meno che non vengono apportate altre modifiche al file.

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>Se replica DFS replica autorizzazioni aggiornate su un file o cartella.

Sì. La replica DFS replica le modifiche alle autorizzazioni per file e cartelle. Viene replicata solo la parte del file associato con l'elenco di controllo di accesso (ACL), anche se replica DFS deve leggere l'intero file in area di gestione temporanea.


> [!NOTE]
> Modifica degli ACL in un numero elevato di file può avere un impatto sulle prestazioni di replica. Tuttavia, quando si utilizza la compressione RDC, la quantità di dati trasferiti è proporzionata alla dimensione degli ACL, non la dimensione dell'intero file. La quantità di traffico su disco è ancora proporzionale alle dimensioni dei file, perché i file devono essere letti da e verso la cartella di gestione temporanea. 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>Replica DFS supporta unendo i file di testo in caso di conflitto?

Replica DFS non unisce i file quando si verifica un conflitto. Tuttavia, prova a mantenere la versione precedente del file nei DfsrPrivate nascosto\\cartelle ConflictandDeleted nel computer in cui è stato rilevato il conflitto.

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>Replica DFS Usa la crittografia durante la trasmissione dei dati?

Sì. Replica DFS utilizza le connessioni RPC Remote Procedure Call () con la crittografia.

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>È possibile disabilitare l'uso di RPC crittografata?

No. Il servizio Replica DFS Usa remote procedure call (RPC) su TCP per la replica dei dati. Per proteggere i trasferimenti di dati attraverso la rete Internet, il servizio Replica DFS è progettato per usare sempre la costante a livello di autenticazione, `RPC_C_AUTHN_LEVEL_PKT_PRIVACY`. Ciò garantisce che le comunicazioni RPC in Internet sono sempre crittografate. Non è pertanto possibile disabilitare l'uso di RPC crittografata dal servizio Replica DFS.

Per altre informazioni, vedere i siti Web Microsoft seguenti:

  - [Riferimento tecnico per RPC](http://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [Sulla compressione differenziale remota](http://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [Costanti dei livelli di autenticazione](http://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>Modalità di gestione repliche simultanee?

È disponibile un gestore di aggiornamento per ogni cartella replicata. Aggiornare il lavoro responsabili indipendentemente uno da altro.

Per impostazione predefinita, un massimo di 16 (quattro in Windows Server 2003 R2) download simultanei vengono condivise tra tutte le connessioni e i gruppi di replica. Poiché le connessioni e gli aggiornamenti di gruppo di replica non vengono serializzati, non vi è alcun ordine specifico in cui vengono ricevuti gli aggiornamenti. Se vengono aperti due pianificazioni, gli aggiornamenti sono in genere ricevuti e installati da entrambe le connessioni allo stesso tempo.

### <a name="how-do-i-force-replication-or-polling"></a>Come forzare la replica o polling?

È possibile forzare la replica immediatamente mediante Gestione DFS, come descritto in [modificare le pianificazioni di replica](https://technet.microsoft.com/en-us/library/Cc732278). È anche possibile forzare la replica usando il `Sync-DfsReplicationGroup` cmdlet, inclusi nel modulo PowerShell di DFSR introdotto con Windows Server 2012 R2, o il **Dfsrdiag SyncNow** comando. È possibile forzare il polling tramite il `Update-DfsrConfigurationFromAD` cmdlet, o il **Dfsrdiag PollAD** comando.

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>È possibile configurare il tempo di attesa tra le repliche per i file modificati di frequente?

No. Se la pianificazione è aperta, replica DFS replicherà le modifiche come li rileva. Non è possibile configurare il tempo di attesa per i file.

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>È possibile configurare la replica unidirezionale con replica DFS?

Sì. Se si usa Windows Server 2012 o Windows Server 2008 R2, è possibile creare una cartella replicata di sola lettura che consente di replicare il contenuto tramite una connessione unidirezionale. Per altre informazioni, vedere [rendere una cartella replicata di sola lettura in un determinato membro](http://go.microsoft.com/fwlink/?linkid=156740) (http://go.microsoft.com/fwlink/?LinkId=156740).

Non è supportata la creazione di una connessione di replica unidirezionale con replica DFS in Windows Server 2008 o Windows Server 2003 R2. Questa operazione può causare numerosi problemi, compresi gli errori di topologia di controllo di integrità, gestione temporanea dei problemi con il database di replica DFS.

Se si usa Windows Server 2008 o Windows Server 2003 R2, è possibile simulare una connessione unidirezionale, eseguendo le azioni seguenti:

  - Eseguire il training agli amministratori di apportare modifiche solo nei server che si vuole designare come server primario. Quindi consentono di che replicano le modifiche apportate ai server di destinazione.  
      
  - Configurare le autorizzazioni di condivisione sul server di destinazione in modo che gli utenti finali non si dispone delle autorizzazioni di scrittura. Se non sono consentite modifiche nei server del ramo, non vi è nulla di cui eseguire la replica, simulazione di una connessione unidirezionale e mantenendo basso utilizzo di WAN.  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>È disponibile un modo per forzare una replica completa di tutti i file inclusi i file non modificati?

No. Se replica DFS considera identici i file, non replicherà li. Se i file modificati non sono stati replicati, replica DFS verranno automaticamente replicati quando configurato per farlo. Per sovrascrivere la pianificazione configurata, utilizzare il metodo WMI **ForceReplicate()** . Si tratta tuttavia solo eseguire l'override di una pianificazione, e non impone la replica dei file invariati o identici.

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>Cosa accade se il membro primario subisce una perdita di database durante la replica iniziale?

Durante la replica iniziale, i file del membro primario avrà sempre la precedenza nella risoluzione dei conflitti che si verifica se i membri di destinazione dispongono di diverse versioni dei file nel membro primario. La designazione di membro primario viene archiviata in Active Directory Domain Services e la designazione viene cancellata dopo il membro primario è pronto per la replica, ma prima di eseguire la replica di tutti i membri della replica di gruppo.

Se la replica iniziale non riesce o viene riavviato il servizio Replica DFS durante la replica, il membro primario vede la designazione di membro primario nel database di replica DFS locale e Ritenta la replica iniziale. Se il database di replica DFS del membro primario viene perso dopo aver cancellato la designazione primaria in servizi di dominio Active Directory, ma prima che tutti i membri del gruppo di replica completare la replica iniziale, tutti i membri del gruppo di replica non è possibile replicare la cartella perché nessun server è designato come membro primario. In questo caso, usare il **Dfsradmin appartenenza/set /isprimary:true** comando sul server membro primario per ripristinare manualmente la designazione di membro primario.

Per altre informazioni sulla replica iniziale, vedere [creare un gruppo di replica](https://technet.microsoft.com/en-us/library/cc725893).


> [!WARNING]
> La designazione di membro primario viene utilizzata solo durante il processo di replica iniziale. Se si usa la <STRONG>Dfsradmin</STRONG> comando per specificare un membro primario per una cartella replicata dopo la replica è stato completato, replica DFS non indicare il server come membro primario in Active Directory Domain Services. Tuttavia, se il database di replica DFS nel server si verifica successivamente irreversibile danneggiamenti o perdite di dati, il server tenta di eseguire una replica iniziale del membro anziché il ripristino dei dati da un altro membro della replica primaria gruppo. In pratica, il server diventa un server primario non autorizzato, che può causare conflitti. Per questo motivo, specificare il membro principale manualmente solo se si è certi che la replica iniziale è irrimediabilmente ha esito negativo. 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>Cosa accade se si chiude la pianificazione della replica anche se un file viene replicato?

Se la compressione differenziale remota (RDC) è abilitata per la connessione, replica di un file di dimensioni superiore a 64 KB che inizia la replica immediatamente prima della chiusura di pianificazione in ingresso (o la modifica a **alcuna larghezza di banda**) continua quando il pianificare apre (o modifiche a un elemento diverso da **alcuna larghezza di banda**). La replica continua dallo stato di cui che si trovava al momento della replica interrotta.

Se la tecnologia RDC è disattivata, replica DFS riavvia completamente il trasferimento di file. Ciò può ritardare quando il file è disponibile nel membro di destinazione.

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>Cosa accade quando due utenti aggiornano contemporaneamente lo stesso file in server diversi?

Quando replica DFS rileva un conflitto, Usa la versione del file che è stato salvato dell'ultima. Sposta l'altri file nei DfsrPrivate\\cartelle ConflictandDeleted (nel percorso locale della cartella replicata nel computer che ha risolto il conflitto). Rimane disponibile fino a Conflict and Deleted la pulizia delle cartelle, che si verifica quando la cartella Conflict and Deleted supera le dimensioni configurate o replica DFS rileva un errore di spazio su disco insufficiente. La cartella Conflict and Deleted non viene replicata e questo metodo di risoluzione dei conflitti evita il problema della directory di u e v che è possibile in FRS.

Quando si verifica un conflitto, replica DFS registra un evento informativo nel registro eventi di replica DFS. Questo evento non richiede l'intervento dell'utente per i motivi seguenti:

  - Non è visibile agli utenti (è visibile solo agli amministratori del server).  
      
  - Replica DFS considera la cartella Conflict and Deleted come una cache. Quando viene raggiunta una soglia di quota, Elimina alcuni di questi file. Non c'è garanzia che verranno salvati i file in conflitto.  
      
  - Il conflitto potrebbe trovarsi in un server diverso rispetto all'origine del conflitto.  
      

## <a name="staging"></a>Gestione temporanea

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>Replica DFS continua quando la replica è disabilitata per una pianificazione o la limitazione delle richieste di quota della larghezza di banda o quando si disabilita manualmente una connessione di gestione temporanea i file?

No. Replica DFS non potrà continuare a file di fase di fuori di volte in cui la replica pianificata, se è stata superata la quota di limitazione della larghezza di banda, oppure quando le connessioni sono disabilitate.

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>Replica DFS non impedisce l'accesso a un file durante la gestione temporanea di altre applicazioni?

No. Replica DFS consente di aprire i file in modo che non blocca utenti o applicazioni di aprire i file nella cartella replica. Questo metodo è noto come "il blocco opportunistico."

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>È possibile modificare il percorso della cartella di gestione temporanea con lo strumento di gestione DFS?

Sì. Il percorso di cartella di gestione temporanea è configurato nel **avanzate** scheda della finestra di **proprietà** finestra di dialogo per ogni membro di un gruppo di replica.

### <a name="when-are-files-staged"></a>Quando vengono gestiti i file?

I file vengono gestiti nel membro del mittente quando il membro di destinazione richiede il file (a meno che il file è di 64 KB o inferiore) come illustrato nella tabella seguente. Se la compressione differenziale remota (RDC) è disabilitato per la connessione, il file viene inserita temporaneamente a meno che non è 256 KB o inferiore. I file vengono anche gestiti nel membro di destinazione durante il trasferimento se sono minori di 64 KB di dimensioni, sebbene sia possibile configurare questa impostazione tra 16 KB e 1 MB. Se la pianificazione è chiusa, i file non vengono gestiti.

### <a name="the-minimum-file-sizes-for-staging-files"></a>Le dimensioni minime per i file di gestione temporanea

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>Connessione desktop remoto abilitato</th>
<th>RDC disabilitata</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>L'invio di membro</p></td>
<td><p>64 KB</p></td>
<td><p>256 KB</p></td>
</tr>
<tr class="odd">
<td><p>Membro di destinazione</p></td>
<td><p>64 KB per impostazione predefinita</p></td>
<td><p>64 KB per impostazione predefinita</p></td>
</tr>
</tbody>
</table>

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>Cosa accade se un file è stato modificato dopo che viene gestita, ma prima di essere completamente trasmesso al sito remoto?

Se qualsiasi parte del file già viene trasmesso, replica DFS continua la trasmissione. Se il file viene modificato prima di trasmettere il file viene avviata la replica DFS, viene inviata la versione più recente del file.

## <a name="change-history"></a>Cronologia delle modifiche


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Date</th>
<th>Descrizione</th>
<th>`Reason`</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>15 novembre 2018</p></td>
<td><p>Aggiornato per Windows Server 2019.</p></td>
<td><p>Nuovo sistema operativo.</p></td>
</tr>
<tr class="even">
<td><p>9 ottobre 2013</p></td>
<td><p>L'aggiornamento di che cosa sono i limiti supportati da di replica DFS? sezione con i risultati di test in Windows Server 2012 R2.</p></td>
<td><p>Aggiornamenti per la versione più recente di Windows Server</p></td>
</tr>
<tr class="odd">
<td><p>Il 30 gennaio 2013</p></td>
<td><p>Aggiunta del servizio Replica DFS viene continuare quando la replica è disabilitata per una pianificazione o la limitazione delle richieste di quota della larghezza di banda o quando si disabilita manualmente una connessione di gestione temporanea i file? voce.</p></td>
<td><p>Domande dei clienti</p></td>
</tr>
<tr class="even">
<td><p>31 ottobre 2012</p></td>
<td><p>Modificare i quali sono i limiti supportati da di replica DFS? voce per aumentare il numero di file replicati in un volume testato.</p></td>
<td><p>Feedback dei clienti</p></td>
</tr>
<tr class="odd">
<td><p>15 agosto 2012</p></td>
<td><p>Modificato NTFS replicano la replica DFS viene file autorizzazioni, flussi di dati alternativi, i collegamenti reali e punti di analisi? punti di ingresso per un'ulteriore chiarire come replica DFS gestisce i collegamenti reali e reparse point.</p></td>
<td><p>Commenti e suggerimenti dal servizio supporto tecnico clienti</p></td>
</tr>
<tr class="even">
<td><p>13 giugno 2012</p></td>
<td><p>Modificare il lavoro viene replica DFS in ReFS o volumi FAT? voce da aggiungere discussione su ReFS.</p></td>
<td><p>Feedback dei clienti</p></td>
</tr>
<tr class="odd">
<td><p>25 aprile 2012</p></td>
<td><p>Modificato NTFS replicano la replica DFS viene file autorizzazioni, flussi di dati alternativi, i collegamenti reali e punti di analisi? voce da chiarire come replica DFS gestisce i collegamenti reali.</p></td>
<td><p>Ridurre la confusione potenziali</p></td>
</tr>
<tr class="even">
<td><p>30 marzo 2011</p></td>
<td><p>Modifica la replica DFS possono replicare Outlook pst o i file di database di Microsoft Office Access? voce per correggere il potenziale impatto della replica DFS con estensione pst e accedere ai file.</p>
<p>Aggiunta come è possibile migliorare le prestazioni della replica?</p></td>
<td><p>Domande dei clienti relative alla voce precedente, che erroneamente indicato che la replica di file pst o accedere ai file potrebbe danneggiare il database di replica DFS.</p></td>
</tr>
<tr class="odd">
<td><p>26 gennaio 2011</p></td>
<td><p>Aggiunto come possano file ripristinati dal ConflictAndDeleted o preesistente cartelle?</p></td>
<td><p>Feedback dei clienti</p></td>
</tr>
<tr class="even">
<td><p>20 ottobre 2010</p></td>
<td><p>Aggiunta come è possibile aggiornare o sostituire un membro di replica DFS?</p></td>
<td><p>Feedback dei clienti</p></td>
</tr>
</tbody>
</table>

