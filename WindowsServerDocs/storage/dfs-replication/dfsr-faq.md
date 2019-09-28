---
title: 'Replica DFS: Domande frequenti'
ms.date: 06/18/2014
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 12410d619245153f759b54e7a8aff257888f04dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386069"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>Replica DFS: Domande frequenti


Aggiornamento: 30 aprile 2019

Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Queste domande frequenti rispondono alle domande sulla replica di file system distribuito (DFS), nota anche come DFS-R o DFSR, per Windows Server.

Per informazioni su Spazi dei nomi DFS, vedere il documento di [domande frequenti su Domande](https://technet.microsoft.com/library/ee404780)frequenti.

Per informazioni sulle novità di Replica DFS, vedere gli argomenti seguenti:

  - [Panoramica di spazi dei nomi DFS e replica DFS](https://technet.microsoft.com/library/jj127250) (in Windows Server 2012)  
      
  - [Novità di file System distribuito](https://technet.microsoft.com/library/ee307957) argomento relativo [alle modifiche della funzionalità da Windows Server 2008 a Windows Server 2008 R2](https://technet.microsoft.com/library/dd391932)  
      
  - [File System distribuito](https://technet.microsoft.com/library/cc753479) argomento nelle [modifiche apportate alle funzionalità da Windows Server 2003 con SP1 a Windows Server 2008](https://technet.microsoft.com/library/cc753208)  
      

Per ottenere un elenco delle modifiche apportate di recente a questo argomento, vedere la sezione [Cronologia delle modifiche](#change-history) di questo argomento.

      

## <a name="interoperability"></a>Interoperabilità

### <a name="can-dfs-replication-communicate-with-frs"></a>È possibile Replica DFS comunicare con FRS?

No. Replica DFS non comunica con il servizio Replica file (FRS). Replica DFS e FRS possono essere eseguiti contemporaneamente nello stesso server, ma non devono mai essere configurati per replicare le stesse cartelle o sottocartelle perché questa operazione può causare la perdita di dati.

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>Può Replica DFS sostituire FRS per la replica SYSVOL

Sì, Replica DFS possibile sostituire FRS per la replica SYSVOL sui server che eseguono Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. I server che eseguono Windows Server 2003 R2 non supportano l'utilizzo di Replica DFS per replicare la cartella SYSVOL.

Per ulteriori informazioni sulla replica di SYSVOL utilizzando replica DFS, vedere la Guida [alla migrazione della replica di SYSVOL: FRS per Replica DFS](https://technet.microsoft.com/library/dd640019).

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>È possibile eseguire l'aggiornamento da FRS a Replica DFS senza perdere le impostazioni di configurazione?

Sì. Per eseguire la migrazione della replica da FRS a Replica DFS, vedere i documenti seguenti:

  - Per eseguire la migrazione della replica di cartelle diverse dalla cartella SYSVOL [, vedere la Guida operativa di DFS: Migrazione da FRS a replica DFS](http://go.microsoft.com/fwlink/?linkid=192776) e [FRS2DFSR: un'utilità di migrazione da FRS a DFSR](http://go.microsoft.com/fwlink/?linkid=195437) (http://go.microsoft.com/fwlink/?LinkID=195437).  
      
  - Per eseguire la migrazione della replica della cartella SYSVOL in replica DFS [, vedere Guida alla migrazione della replica di SYSVOL: FRS per Replica DFS](https://technet.microsoft.com/library/dd640019).  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>È possibile utilizzare Replica DFS in un ambiente misto Windows/UNIX?

Sì. Sebbene Replica DFS supporti solo la replica di contenuto tra server che eseguono Windows Server, i client UNIX possono accedere alle condivisioni file nei server Windows. A tale scopo, installare i servizi per NFS (Network File System) nel server Replica DFS.

È inoltre possibile utilizzare la funzionalità client SMB/CIFS inclusa in molti client UNIX per accedere direttamente alle condivisioni file di Windows, anche se questa funzionalità è spesso limitata o richiede modifiche all'ambiente Windows, ad esempio la disabilitazione della firma SMB tramite Criteri di gruppo).

Replica DFS interagisce con NFS in un server che esegue un sistema operativo Windows Server, ma non è possibile replicare un punto di montaggio NFS.

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>È possibile usare la Servizio Copia Shadow del volume con Replica DFS?

Sì. Replica DFS è supportata nei volumi di Servizio Copia Shadow del volume (VSS) ed è possibile ripristinare correttamente gli snapshot precedenti con il client versioni precedenti.

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>È possibile utilizzare Windows backup (Ntbackup. exe) per eseguire il backup remoto di una cartella replicata?

No, l'uso di Windows backup (Ntbackup. exe) in un computer che esegue Windows Server 2003 o versioni precedenti per eseguire il backup del contenuto di una cartella replicata in un computer che esegue Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 non è supportato.

Per eseguire il backup dei file archiviati in una cartella replicata, utilizzare Windows Server Backup o Microsoft® System Center Data Protection Manager. Per informazioni sulle funzionalità di backup e ripristino in Windows Server 2008 R2 e Windows Server 2008, vedere [backup e ripristino](https://technet.microsoft.com/library/Cc754097). Per ulteriori informazioni, vedere [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=182261) (http://go.microsoft.com/fwlink/?LinkId=182261).

### <a name="do-file-system-policies-impact-dfs-replication"></a>I criteri di file system influiscano Replica DFS?

Sì. Non configurare i criteri di file system per le cartelle replicate. Il criterio di file system riapplica le autorizzazioni NTFS a ogni Criteri di gruppo intervallo di aggiornamento. Questo può comportare violazioni della condivisione perché un file aperto non viene replicato fino alla chiusura del file.

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>Replica DFS replicare le cassette postali ospitate su Microsoft Exchange Server?

No. Impossibile utilizzare Replica DFS per replicare le cassette postali ospitate su Microsoft Exchange Server.

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>Replica DFS supporta le schermate dei file create dal file server Gestione risorse?

Sì. Tuttavia, le impostazioni di screening dei file Gestione risorse (FSRM) del file server devono corrispondere a entrambe le estremità della replica. Inoltre, Replica DFS dispone di un proprio meccanismo di filtro per i file e le cartelle che è possibile utilizzare per escludere determinati file e tipi di file dalla replica.

Di seguito sono riportate le procedure consigliate per l'implementazione di schermate file o quote:

  - La cartella nascosta DfsrPrivate non deve essere soggetta a quote o schermate di file.  
      
  - Prima di abilitare la selezione, i file selezionati non devono esistere in alcuna cartella replicata.  
      
  - Nessuna cartella può superare la quota prima che la quota sia abilitata.  
      
  - È necessario usare le quote rigide con cautela. È possibile che i singoli membri di un gruppo di replica restino all'interno di una quota prima della replica, ma lo superino quando i file vengono replicati. Se, ad esempio, un utente copia un file di 10 megabyte (MB) nel server A (che è quindi al limite rigido) e un altro utente copia un file di 5 MB sul server B, quando si verifica la replica successiva, entrambi i server superano la quota di 5 megabyte. Questa operazione può causare la ripetizione continua della replica dei file Replica DFS, causando buchi nel vettore di versione e possibili problemi di prestazioni.  
      

### <a name="is-dfs-replication-cluster-aware"></a>Il cluster è in grado di riconoscere Replica DFS?

Sì, Replica DFS in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2 includono la possibilità di aggiungere un cluster di failover come membro di un gruppo di replica. Per ulteriori informazioni, vedere [aggiungere un cluster di failover a un gruppo di replica](http://go.microsoft.com/fwlink/?linkid=155085) (http://go.microsoft.com/fwlink/?LinkId=155085). Il servizio Replica DFS nelle versioni di Windows precedenti a Windows Server 2008 R2 non è progettato per coordinarsi con un cluster di failover e il servizio non eseguirà il failover su un altro nodo.


> [!NOTE]
> Replica DFS non supporta la replica di file in volumi condivisi cluster. 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>Replica DFS è compatibile con la deduplicazione dei dati?

Sì, Replica DFS possibile replicare le cartelle in volumi che usano la deduplicazione dati in Windows Server.

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>Replica DFS è compatibile con RIS e WDS?

Sì. Replica DFS replica i volumi in cui è abilitata l'archiviazione a istanza singola (SIS). SIS viene usato da servizi di installazione remota (RIS), servizi di distribuzione Windows (WDS) e Windows Storage Server.

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>È possibile usare Replica DFS con File offline?

È possibile utilizzare in modo sicuro Replica DFS e File offline insieme in scenari in cui è presente un solo utente alla volta che scrive nei file. Questa operazione è utile per gli utenti che si spostano tra due succursali e vogliono poter accedere ai file in un ramo o in modalità offline. File offline memorizza nella cache i file in locale per l'utilizzo offline e Replica DFS replica i dati tra le succursali.

Non utilizzare Replica DFS con File offline in un ambiente multiutente, perché Replica DFS non fornisce alcun meccanismo di blocco distribuito o funzionalità di estrazione dei file. Se due utenti modificano lo stesso file contemporaneamente in server diversi, replica DFS sposta il file precedente nella cartella DfsrPrivate\\cartella ConflictAndDeleted (che si trova nel percorso locale della cartella replicata) durante la replica successiva.

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>Quali applicazioni antivirus sono compatibili con Replica DFS?

Le applicazioni antivirus possono causare una replica eccessiva se le attività di analisi modificano i file in una cartella replicata. Per ulteriori informazioni, [testare l'interoperabilità delle applicazioni antivirus con replica DFS](http://go.microsoft.com/fwlink/?linkid=73990) (http://go.microsoft.com/fwlink/?LinkId=73990).

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>Quali sono i vantaggi dell'utilizzo di Replica DFS anziché Windows SharePoint Services?

Windows® SharePoint® Services fornisce un coerenza rigoroso sotto forma di funzionalità di estrazione dei file che Replica DFS No. Se si è interessati a più persone che modificano lo stesso file, è consigliabile utilizzare Windows SharePoint Services. Windows SharePoint Services 2,0 con Service Pack 2 è disponibile come parte di Windows Server 2003 R2. Windows SharePoint Services può essere scaricato dal sito Web Microsoft; non è incluso nelle versioni più recenti di Windows Server. Tuttavia, se si esegue la replica dei dati in più siti e gli utenti non modificheranno contemporaneamente gli stessi file, Replica DFS garantisce una maggiore larghezza di banda e una gestione più semplice.

## <a name="limitations-and-requirements"></a>Limitazioni e requisiti

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>È Replica DFS possibile eseguire la replica tra succursali senza una connessione VPN?

Sì, supponendo che sia presente un collegamento WAN (Wide Area Network) privato (non Internet) che connette le succursali. Tuttavia, è necessario aprire le porte appropriate nei firewall esterni. Replica DFS utilizza l'agente mapping endpoint RPC (porta 135) e una porta temporanea assegnata in modo casuale sopra 1024. È possibile usare lo strumento da riga di comando **Dfsrdiag** per specificare una porta statica invece della porta temporanea. Per ulteriori informazioni su come specificare il mapper di endpoint RPC, vedere l' [articolo 154596](http://go.microsoft.com/fwlink/?linkid=73991) nella Microsoft Knowledge base (http://go.microsoft.com/fwlink/?LinkId=73991).

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>È possibile Replica DFS replicare i file crittografati con i Encrypting File System?

No. Replica DFS non eseguirà la replica di file o cartelle crittografati con Encrypting File System (EFS). Se un utente crittografa un file precedentemente replicato, Replica DFS Elimina il file da tutti gli altri membri del gruppo di replica. In questo modo si garantisce che l'unica copia disponibile del file sia la versione crittografata sul server.

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>È possibile Replica DFS replicare i file di database di Outlook. pst o Microsoft Office Access?

Replica DFS possibile eseguire la replica in modo sicuro dei file di cartelle personali di Microsoft Outlook (pst) e di Microsoft Access solo se vengono archiviati per scopi di archiviazione e non sono accessibili attraverso la rete tramite un client come Outlook o Access (per aprire. pst o Access file, copiare prima i file in un dispositivo di archiviazione locale). I motivi sono i seguenti:

  - L'apertura dei file. pst su connessioni di rete potrebbe causare il danneggiamento dei dati nei file. pst. Per ulteriori informazioni sui motivi per cui non è possibile accedere in modo sicuro ai file con estensione pst da una rete, vedere l' [articolo 297019](http://go.microsoft.com/fwlink/?linkid=125363) della Microsoft Knowledge base (http://go.microsoft.com/fwlink/?LinkId=125363).  
      
  - i file con estensione pst e Access tendono a rimanere aperti per lunghi periodi di tempo durante l'accesso da un client come Outlook o Office Access. In questo modo si impedisce a Replica DFS di replicare questi file fino a quando non vengono chiusi.  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>È possibile utilizzare Replica DFS in un gruppo di lavoro?

No. Replica DFS si basa su Active Directory® Domain Services per la configurazione. Funzionerà solo in un dominio.

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>È possibile replicare più di una cartella in un singolo server?

Sì. Replica DFS possibile replicare numerose cartelle tra server. Verificare che ogni cartella replicata disponga di un percorso radice univoco e che non si sovrappongano. Ad esempio, d:\\Sales e d:\\contabilità possono essere i percorsi radice per due cartelle replicate, mentre i report\\d: Sales e\\d\\: Sales non possono essere i percorsi radice per due cartelle replicate.

### <a name="does-dfs-replication-require-dfs-namespaces"></a>Replica DFS richiedono spazi dei nomi DFS?

No. Gli spazi dei nomi Replica DFS e DFS possono essere utilizzati separatamente o insieme. Inoltre, è possibile utilizzare Replica DFS per replicare spazi dei nomi DFS autonomi, operazione non possibile con FRS.

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>Replica DFS richiede la sincronizzazione dell'ora tra i server?

No. Replica DFS non richiede in modo esplicito la sincronizzazione dell'ora tra i server. Tuttavia, Replica DFS richiede che gli orologi del server corrispondano attentamente. Per il corretto funzionamento dell'autenticazione Kerberos, gli orologi del server devono essere impostati entro cinque minuti l'uno dall'altro (per impostazione predefinita). Ad esempio, Replica DFS usa i timestamp per determinare quale file ha la precedenza in caso di conflitto. Gli orari accurati sono importanti anche per Garbage Collection, pianificazioni e altre funzionalità.

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>Replica DFS supporta la replica di un intero volume?

Sì. Tuttavia, è necessario innanzitutto installare Windows Server 2003 Service Pack 2 o l'hotfix. Per ulteriori informazioni, vedere l' [articolo 920335](http://go.microsoft.com/fwlink/?linkid=76776) della Microsoft Knowledge base (http://go.microsoft.com/fwlink/?LinkId=76776). Inoltre, la replica di un intero volume può causare i problemi seguenti:

  - Se il volume contiene un file di paging di Windows, la replica ha esito negativo e registra l'evento DFSR 4312 nel registro eventi di sistema.  
      
  - Replica DFS imposta il sistema e gli attributi nascosti nella cartella replicata nei server di destinazione. Questo problema si verifica perché Windows applica gli attributi System e Hidden alla cartella radice del volume per impostazione predefinita. Se il percorso locale della cartella replicata nei server di destinazione è anche una radice del volume, non vengono apportate altre modifiche agli attributi della cartella.  
      
  - Quando si esegue la replica di un volume che contiene la cartella di sistema di Windows, Replica DFS riconosce la cartella% WINDIR% e non la replica. Tuttavia, Replica DFS replica le cartelle utilizzate dalle applicazioni non Microsoft, che potrebbero causare errori nelle applicazioni nei server di destinazione se le applicazioni presentano problemi di interoperabilità con Replica DFS.  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>Replica DFS supporta RPC su HTTP?

No.

### <a name="does-dfs-replication-work-across-wireless-networks"></a>Replica DFS funziona nelle reti wireless?

Sì. Replica DFS è indipendente dal tipo di connessione.

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>Replica DFS funziona sui volumi ReFS o FAT?

No. Replica DFS supporta solo volumi formattati con il file system NTFS. il file system Resilient file System (ReFS) e il FAT non sono supportati. Replica DFS richiede NTFS perché utilizza il Journal delle modifiche NTFS e altre funzionalità del file system NTFS.

### <a name="does-dfs-replication-work-with-sparse-files"></a>Replica DFS funziona con i file sparse?

Sì. È possibile replicare file sparse. L'attributo **sparse** viene mantenuto nel membro ricevente.

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>È necessario accedere come amministratore per replicare i file?

No. Replica DFS è un servizio eseguito con l'account di sistema locale, pertanto non è necessario accedere come amministratore per eseguire la replica. È tuttavia necessario essere un amministratore di dominio o locale dei file server interessati per apportare modifiche alla configurazione del Replica DFS.

Per ulteriori informazioni, vedere "Replica DFS requisiti di sicurezza e delega" nel [delegare la capacità di gestire replica DFS](http://go.microsoft.com/fwlink/?linkid=182294) (http://go.microsoft.com/fwlink/?LinkId=182294).

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>Come è possibile aggiornare o sostituire un membro Replica DFS?

Per aggiornare o sostituire un membro di Replica DFS, vedere questo post di Blog sul Blog del team di Ask the Directory Services: [Sostituzione dell'hardware o del sistema operativo del membro DFSR](http://blogs.technet.com/b/askds/archive/2010/09/10/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os.aspx).

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>Replica DFS è adatto per la replica di profili mobili?

Sì. Alcuni scenari sono supportati quando si replicano i profili utente mobili. Per informazioni sugli scenari supportati, vedere la pagina relativa all' [istruzione di supporto di Microsoft relativa ai dati del profilo utente replicati](http://go.microsoft.com/fwlink/?linkid=201282) (http://go.microsoft.com/fwlink/?LinkId=201282).

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>È previsto un limite di caratteri di file o un limite alla profondità della cartella?

Windows e Replica DFS supportano percorsi di cartelle con un massimo di 32000 caratteri. Replica DFS non è limitato ai percorsi di cartella di 260 caratteri.

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>I membri di un gruppo di replica devono trovarsi nello stesso dominio?

No. I gruppi di replica possono estendersi tra domini all'interno di una singola foresta ma non tra foreste diverse.

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>Quali sono i limiti supportati di Replica DFS?

L'elenco seguente fornisce un set di linee guida per la scalabilità che sono state testate da Microsoft in Windows Server 2012 R2:

  - Dimensioni di tutti i file replicati in un server: 100 terabyte.  
      
  - Numero di file replicati in un volume: 70 milioni.  
      
  - Dimensioni massime file: 250 gigabyte.  
      


> [!IMPORTANT]
> Quando si creano gruppi di replica con un numero elevato di file o dimensioni, è consigliabile esportare un clone del database e usare tecniche di pre-seeding per ridurre al minimo la durata della replica iniziale. Per ulteriori informazioni, vedere <A href="http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx">replica DFS sincronizzazione iniziale in Windows Server 2012 R2: Attacco dei cloni</A>. 
<br>


L'elenco seguente include un set di linee guida per la scalabilità che sono state testate da Microsoft in Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008:

  - Dimensioni di tutti i file replicati in un server: 10 terabyte.  
      
  - Numero di file replicati in un volume: 11 milioni.  
      
  - Dimensioni massime file: 64 gigabyte.  
      


> [!NOTE]
> Non esiste più un limite al numero di gruppi di replica, cartelle replicate, connessioni o membri del gruppo di replica. 
<br>


Per un elenco delle linee guida per la scalabilità testate da Microsoft per Windows Server 2003 R2, vedere [replica DFS linee guida sulla scalabilità](http://go.microsoft.com/fwlink/?linkid=75043) (http://go.microsoft.com/fwlink/?LinkId=75043).

### <a name="when-should-i-not-use-dfs-replication"></a>Quando non è consigliabile usare Replica DFS?

Non utilizzare Replica DFS in un ambiente in cui più utenti aggiornano o modificano contemporaneamente gli stessi file in server diversi. Questa operazione può causare la replica DFS di spostare copie in conflitto dei file nella cartella DfsrPrivate\\cartella ConflictAndDeleted nascosta.

Quando più utenti devono modificare contemporaneamente gli stessi file in server diversi, utilizzare la funzionalità di estrazione dei file di Windows SharePoint Services per garantire che un solo utente stia lavorando su un file. Windows SharePoint Services 2,0 con Service Pack 2 è disponibile come parte di Windows Server 2003 R2. Windows SharePoint Services può essere scaricato dal sito Web Microsoft; non è incluso nelle versioni più recenti di Windows Server.

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>Perché è necessario un aggiornamento dello schema per Replica DFS?

Replica DFS usa nuovi oggetti nel contesto dei nomi di dominio di Active Directory Domain Services per archiviare le informazioni di configurazione. Questi oggetti vengono creati quando si aggiorna lo schema di Active Directory Domain Services. Per ulteriori informazioni, vedere [la pagina relativa ai requisiti di revisione per replica DFS](http://go.microsoft.com/fwlink/?linkid=182264) (http://go.microsoft.com/fwlink/?LinkId=182264).

## <a name="monitoring-and-management-tools"></a>Strumenti di monitoraggio e gestione

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>È possibile automatizzare il rapporto di stato per ricevere avvisi?

Sì. Sono disponibili tre modi per automatizzare i rapporti di stato:

  - Usare il modulo DFSR di Windows PowerShell incluso in Windows Server 2012 R2 o DfsrAdmin. exe insieme alle attività pianificate per generare regolarmente report sull'integrità. Per ulteriori informazioni, vedere [automazione di replica DFS report](http://go.microsoft.com/fwlink/?linkid=74010) sull'integrità http://go.microsoft.com/fwlink/?LinkId=74010) (.  
      
  - Utilizzare il Management Pack Replica DFS per System Center Operations Manager per creare avvisi basati su condizioni specificate.  
      
  - Utilizzare il provider WMI Replica DFS per creare script per gli avvisi.  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>È possibile utilizzare Microsoft System Center Operations Manager per monitorare Replica DFS?

Sì. Per ulteriori informazioni, vedere il [Management Pack replica DFS per System Center Operations Manager 2007](http://go.microsoft.com/fwlink/?linkid=182265) nell'area download Microsoft (http://go.microsoft.com/fwlink/?LinkId=182265).

### <a name="does-dfs-replication-support-remote-management"></a>Replica DFS supporta la gestione remota?

Sì. Replica DFS supporta la gestione remota tramite la console di gestione DFS e il comando **Aggiungi gruppo di replica** . Ad esempio, nel server A è possibile connettersi a un gruppo di replica definito nella foresta con i server A e B come membri.

Gestione DFS è incluso in Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 e Windows Server 2003 R2. Per gestire Replica DFS da altre versioni di Windows, usare Desktop remoto o la [strumenti di amministrazione remota del server per Windows 7](https://technet.microsoft.com/library/Ee449475).


> [!IMPORTANT]
> Per visualizzare o gestire i gruppi di replica che contengono cartelle replicate di sola lettura o membri che sono cluster di failover, è necessario utilizzare la versione di gestione DFS inclusa in Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, <a href="http://go.microsoft.com/fwlink/p/?linkid=238560">Remote Strumenti di amministrazione del server per Windows 8</a>o il <a href="https://technet.microsoft.com/library/ee449475">strumenti di amministrazione remota del server per Windows 7</a>. 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>Gli ultrasuoni e i sonar funzionano con Replica DFS?

No. Replica DFS dispone di un proprio set di strumenti di monitoraggio e diagnostica. Ultrasuoni e sonar sono in grado di monitorare solo il servizio Replica file.

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>In che modo è possibile ripristinare i file dalle cartelle cartella ConflictAndDeleted o preesistenti?

Per ripristinare i file persi, ripristinare i file dalla cartella file system o dalla cartella condivisa usando la cronologia file, il comando **Ripristina versioni precedenti** in Esplora file o ripristinando i file dal backup. Per ripristinare i file direttamente dalla cartella cartella ConflictAndDeleted o PreExisting, usare `Get-DfsrPreservedFiles` i `Restore-DfsrPreservedFiles` cmdlet di e Windows PowerShell (inclusi nel modulo DFSR in Windows Server 2012 R2) o lo script di esempio [RestoreDFSR](http://code.msdn.microsoft.com/restoredfsr) da MSDN Raccolta di codice. Questo script è destinato esclusivamente al ripristino di emergenza e viene fornito così com'è, senza garanzie.

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>Esiste un modo per comprendere lo stato della replica?

Sì. Sono disponibili diversi modi per monitorare la replica:

  - Replica DFS dispone di un Management Pack per System Center Operations Manager che fornisce il monitoraggio proattivo.  
      
  - Gestione DFS include un report di diagnostica interno per il backlog di replica, l'efficienza della replica e il numero di file e cartelle in un determinato gruppo di replica.  
      
  - Il modulo DFSR di Windows PowerShell in Windows Server 2012 R2 contiene i cmdlet per l'avvio dei test di propagazione e la scrittura di report di propagazione e integrità. Per altre informazioni, vedere [file System distribuito cmdlet di replica in Windows PowerShell](https://technet.microsoft.com/library/dn296601.aspx).  
      
  - Dfsrdiag. exe è uno strumento da riga di comando che può generare un numero di backlog o attivare un test di propagazione. Entrambi mostrano lo stato della replica. La propagazione Mostra se i file vengono replicati in tutti i nodi. Il backlog Mostra il numero di file che devono ancora essere replicati prima che due computer siano sincronizzati. Il conteggio dei backlog è il numero di aggiornamenti non elaborati da un membro del gruppo di replica. Nei computer che eseguono Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2, Dfsrdiag. exe è in grado di visualizzare anche gli aggiornamenti che Replica DFS sta attualmente eseguendo la replica.  
      
  - Gli script possono utilizzare WMI per raccogliere informazioni sul backlog, manualmente o tramite MOM.  
      

## <a name="performance"></a>Prestazioni

### <a name="does-dfs-replication-support-dial-up-connections"></a>Replica DFS supportano le connessioni remote?

Anche se Replica DFS funzionerà a velocità di connessione remota, è possibile che venga eseguito il backlog se è presente un numero elevato di modifiche da replicare. Se vengono apportate piccole modifiche ai file esistenti, Replica DFS con la compressione differenziale remota (RDC) fornirà prestazioni molto più elevate rispetto alla copia diretta del file.

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>Replica DFS esegue il rilevamento della larghezza di banda?

No. Replica DFS non esegue il rilevamento della larghezza di banda. È possibile configurare Replica DFS per l'uso di una quantità limitata di larghezza di banda in base alla connessione (limitazione della larghezza di banda). Tuttavia, Replica DFS non riduce ulteriormente l'utilizzo della larghezza di banda se l'interfaccia di rete viene saturata e Replica DFS può saturare il collegamento per brevi periodi. La limitazione della larghezza di banda con Replica DFS non è completamente precisa perché Replica DFS limita la larghezza di banda tramite la limitazione delle chiamate RPC. Di conseguenza, vari buffer in livelli inferiori dello stack di rete (inclusa la RPC) potrebbero interferire, causando picchi di traffico di rete.

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>Replica DFS limita la larghezza di banda per ogni pianificazione, per server o per connessione?

Se si configura la limitazione della larghezza di banda quando si specifica la pianificazione, tutte le connessioni per il gruppo di replica utilizzeranno tale impostazione per la limitazione della larghezza di banda. La limitazione della larghezza di banda può essere impostata anche come impostazione a livello di connessione tramite Gestione DFS.

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>Replica DFS utilizza Active Directory Domain Services per calcolare i collegamenti di sito e i costi di connessione?

No. Replica DFS utilizza la topologia definita dall'amministratore, indipendente da Active Directory Domain Services il costo del sito.

### <a name="how-can-i-improve-replication-performance"></a>Come è possibile migliorare le prestazioni della replica?

Per informazioni sui diversi metodi di ottimizzazione delle prestazioni della replica, vedere l'articolo relativo all' [ottimizzazione delle prestazioni della replica in DFSR](http://blogs.technet.com/b/askds/archive/2010/03/31/tuning-replication-performance-in-dfsr-especially-on-win2008-r2.aspx) nel [Blog del team Ask the Directory Services](http://blogs.technet.com/b/askds/).

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>In che modo Replica DFS evitare di saturare una connessione?

In Replica DFS è possibile impostare la larghezza di banda massima che si desidera utilizzare in una connessione e il servizio mantiene tale livello di utilizzo della rete. Questa operazione è diversa da quella del Servizio trasferimento intelligente in background (BITS) e Replica DFS non satura la connessione se impostata in modo appropriato.

Tuttavia, la limitazione della larghezza di banda non è accurata del 100% e Replica DFS può saturare il collegamento per brevi periodi di tempo. Questo perché Replica DFS limita la larghezza di banda tramite la limitazione delle chiamate RPC. Poiché questo processo si basa su vari buffer nei livelli inferiori dello stack di rete, inclusa la RPC, il traffico di replica tende a spostarsi in picchi che potrebbero a volte saturare i collegamenti di rete.

Replica DFS in Windows Server 2008 include diversi miglioramenti delle prestazioni, come descritto in [file System distribuito](https://technet.microsoft.com/library/Cc753479), un argomento relativo [alle modifiche della funzionalità da Windows Server 2003 con SP1 a Windows Server 2008](https://technet.microsoft.com/library/cc753208).

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>Come vengono confrontate le prestazioni Replica DFS con FRS?

Replica DFS è molto più veloce di FRS, in particolare quando vengono apportate piccole modifiche a file di grandi dimensioni e la tecnologia RDC è abilitata. Con RDC, ad esempio, una piccola modifica a una presentazione di PowerPoint® da 2 MB può produrre solo 60 kilobyte (KB) inviati attraverso la rete, ovvero un risparmio del 97% in byte trasferiti.

RDC non viene utilizzato su file di dimensioni inferiori a 64 KB e potrebbe non essere vantaggioso per le reti LAN ad alta velocità in cui la larghezza di banda di rete non è contesa. La tecnologia RDC può essere disabilitata per ogni singola connessione mediante Gestione DFS.

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>Con quale frequenza Replica DFS replicare i dati?

I dati vengono replicati in base alla pianificazione impostata. Ad esempio, è possibile impostare la pianificazione su intervalli di 15 minuti, sette giorni alla settimana. Durante questi intervalli, la replica è abilitata. La replica viene avviata subito dopo che è stata rilevata una modifica di file (in genere in secondi)

La pianificazione del gruppo di replica può essere impostata su UTC (Universal Time Coordinate) mentre la pianificazione della connessione è impostata sull'ora locale del membro di destinazione. Prendere in considerazione questo problema quando il gruppo di replica si estende su più fusi orari. Ora locale indica l'ora del membro che ospita la connessione in ingresso. La pianificazione visualizzata della connessione in ingresso e la corrispondente connessione in uscita riflettono le differenze di fuso orario quando la pianificazione è impostata sull'ora locale.

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>Qual è la quantità di risorse di sistema del server che Replica DFS utilizzerà?

Le risorse del disco, della memoria e della CPU utilizzate da Replica DFS dipendono da diversi fattori, tra cui il numero e le dimensioni dei file, la frequenza di modifica, il numero di membri del gruppo di replica e il numero di cartelle replicate. Inoltre, alcune risorse sono più difficili da stimare. Ad esempio, la tecnologia Extensible Storage Engine (ESE) utilizzata per il database di Replica DFS può utilizzare un'alta percentuale di memoria disponibile, che rilascia su richiesta. Le applicazioni diverse da Replica DFS possono essere ospitate nello stesso server a seconda della configurazione del server. Tuttavia, quando si ospitano più applicazioni o ruoli del server in un singolo server, è importante testare questa configurazione prima di implementarla in un ambiente di produzione.

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>Cosa accade se un collegamento WAN non riesce durante la replica?

Se la connessione viene interrotta, Replica DFS continuerà a tentare la replica mentre la pianificazione è aperta. Si verificano inoltre errori di connettività indicati nel registro eventi di Replica DFS che possono essere raccolti utilizzando MOM (in modo proattivo tramite avvisi) e il rapporto di stato di Replica DFS (in modo reattivo, ad esempio quando viene eseguito da un amministratore).

## <a name="remote-differential-compression-details"></a>Dettagli di compressione differenziale remota

### <a name="are-changes-compressed-before-being-replicated"></a>Le modifiche vengono compresse prima di essere replicate?

Sì. Le parti modificate dei file vengono compresse prima di essere inviate per tutti i tipi di file, eccetto le seguenti (che sono già compresse):. WMA, WMV, zip, jpg, MPG, MPEG,. m1v,. MP2,. mp3,. MPa,. cab,. wav,. snd,. au,. ASF,. WM,. avi,. z,. gz,. tgz e. FRx. Le impostazioni di compressione per questi tipi di file non sono configurabili in Windows Server 2003 R2.

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>Un amministratore può disattivare la tecnologia RDC o modificare la soglia?

Sì. È possibile disattivare RDC tramite la pagina delle proprietà di una determinata connessione. La disabilitazione di RDC può ridurre l'utilizzo della CPU e la latenza di replica sui collegamenti della rete locale (LAN) veloci senza vincoli di larghezza di banda o per i gruppi di replica che sono costituiti principalmente da file di dimensioni inferiori a 64 KB. Se si sceglie di disabilitare RDC in una connessione, testare l'efficienza della replica prima e dopo la modifica per verificare di avere migliorato le prestazioni di replica.

È possibile modificare la soglia delle dimensioni RDC usando il comando **DfsrAdmin Connection set** , il Provider WMI replica DFS o modificando manualmente il file XML di configurazione.

### <a name="does-rdc-work-on-all-file-types"></a>RDC funziona su tutti i tipi di file?

Sì. RDC calcola le differenze a livello di blocco indipendentemente dal tipo di dati del file. Tuttavia, RDC funziona in modo più efficiente in determinati tipi di file, ad esempio documenti di Word, file PST e immagini VHD.

### <a name="how-does-rdc-work-on-a-compressed-file"></a>Come funziona RDC in un file compresso?

Replica DFS utilizza RDC, che consente di calcolare i blocchi nel file che sono stati modificati e di inviare solo i blocchi sulla rete. Replica DFS non è necessario conoscere il contenuto del file, ma solo i blocchi che sono stati modificati.

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>La RDC tra file è abilitata durante l'aggiornamento a Windows Server Enterprise Edition o Datacenter Edition?

Le edizioni standard di Windows Server non supportano la RDC tra file. Tuttavia, viene abilitato automaticamente quando si esegue l'aggiornamento a un'edizione che supporta la RDC tra file o se un membro della connessione di replica esegue un'edizione supportata. Per un elenco di edizioni che supportano la RDC tra file, vedere quali edizioni del sistema operativo Windows supportano la RDC tra file?

### <a name="is-rdc-true-block-level-replication"></a>La replica è true a livello di blocco di RDC?

No. RDC è un protocollo per utilizzo generico per la compressione del trasferimento di file. Replica DFS USA RDC su blocchi a livello di file, non a livello di blocco del disco. RDC divide un file in blocchi. Per ogni blocco in un file, viene calcolata una firma, ovvero un numero ridotto di byte che può rappresentare il blocco più grande. Il set di firme viene trasferito da server a client. Il client confronta le firme del server con le proprie. Il client richiede quindi al server di inviare solo i dati per le firme che non sono già presenti nel client.

### <a name="what-happens-if-i-rename-a-file"></a>Che cosa accade se si rinomina un file?

Replica DFS Rinomina il file in tutti gli altri membri del gruppo di replica durante la successiva replica. I file vengono rilevati con un ID univoco, quindi la ridenominazione di un file e lo scorrimento di un file all'interno della replica non influisce sulla capacità di Replica DFS di replicare un file.

### <a name="what-is-cross-file-rdc"></a>Che cos'è la RDC tra file?

La tecnologia RDC tra file consente Replica DFS di utilizzare RDC anche quando non esiste un file con lo stesso nome nel lato client. RDC tra file usa un'euristica per determinare i file che sono simili al file che deve essere replicato e usa blocchi di file simili identici al file di replica per ridurre al minimo la quantità di dati trasferiti sulla rete WAN. La RDC tra file può usare blocchi di un massimo di cinque file simili in questo processo.

Per utilizzare la RDC tra file, un membro della connessione di replica deve eseguire un'edizione di Windows che supporta la RDC tra file. Per un elenco di edizioni che supportano la RDC tra file, vedere quali edizioni del sistema operativo Windows supportano la RDC tra file?

### <a name="what-is-rdc"></a>Che cos'è RDC?

La compressione differenziale remota (RDC) è un protocollo client-server che può essere usato per aggiornare in modo efficiente i file in una rete con larghezza di banda limitata. RDC rileva inserimenti, rimozioni e riarrangiamenti dei dati nei file, consentendo Replica DFS di replicare solo le modifiche apportate quando i file vengono aggiornati. La tecnologia RDC viene utilizzata solo per i file di dimensioni pari o superiori a 64 KB per impostazione predefinita. RDC può utilizzare una versione precedente di un file con lo stesso nome nella cartella replicata o nella cartella DfsrPrivate\\cartella ConflictAndDeleted (che si trova nel percorso locale della cartella replicata).

### <a name="when-is-rdc-used-for-replication"></a>Quando viene utilizzata la tecnologia RDC per la replica?

La tecnologia RDC viene utilizzata quando il file supera una soglia di dimensioni minime. Per impostazione predefinita, questa soglia delle dimensioni è 64 KB. Dopo che un file che supera tale soglia è stato replicato, le versioni aggiornate del file utilizzano sempre RDC, a meno che non venga modificata una parte grande del file o la RDC sia disabilitata.

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>Quali edizioni del sistema operativo Windows supportano la RDC tra file?

Per utilizzare la RDC tra file, un membro della connessione di replica deve eseguire un'edizione del sistema operativo Windows che supporta la RDC tra file. Nella tabella seguente vengono illustrate le edizioni del sistema operativo Windows che supportano la RDC tra file.

### <a name="cross-file-rdc-availability-in-editions-of-the-windows-operating-system"></a>Disponibilità di RDC tra file nelle edizioni del sistema operativo Windows

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

\*Facoltativamente, è possibile disabilitare la RDC tra file in Windows Server 2012 R2.

## <a name="replication-details"></a>Dettagli replica

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>È possibile modificare il percorso di una cartella replicata dopo che è stata creata?

No. Se è necessario modificare il percorso di una cartella replicata, è necessario eliminarla in Gestione DFS e aggiungerla nuovamente come nuova cartella replicata. Replica DFS quindi utilizza la compressione differenziale remota (RDC) per eseguire una sincronizzazione che determina se i dati sono gli stessi per i membri di invio e ricezione. Non esegue di nuovo la replica di tutti i dati nella cartella.

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>È possibile configurare gli attributi di file da replicare?

No, non è possibile configurare gli attributi di file che Replica DFS replica.

Per un elenco di valori di attributo e delle relative descrizioni, vedere [attributi](http://go.microsoft.com/fwlink/?linkid=182268) di file http://go.microsoft.com/fwlink/?LinkId=182268) su MSDN (.

I valori di attributo seguenti vengono impostati tramite la `SetFileAttributes dwFileAttributes` funzione e vengono replicati dal replica DFS. Le modifiche apportate a questi valori di attributo attivano la replica degli attributi. Il contenuto del file non viene replicato, a meno che non si modifichi anche il contenuto. Per ulteriori informazioni, vedere la [funzione SetFileAttributes](http://go.microsoft.com/fwlink/?linkid=182269) in MSDN Library (http://go.microsoft.com/fwlink/?LinkId=182269).

  - \_ATTRIBUTO\_FILE NASCOSTO  
      
  - \_ATTRIBUTO\_FILE READONLY  
      
  - SISTEMA\_DI\_ATTRIBUTI DI FILE  
      
  - ATTRIBUTO\_FILE\_NONCONTENUTO\_INDICIZZATO\_  
      
  - \_ATTRIBUTO\_FILE OFFLINE  
      

I valori di attributo seguenti vengono replicati dal Replica DFS, ma non attivano la replica.

  - ARCHIVIO\_ATTRIBUTI\_FILE  
      
  - \_ATTRIBUTO\_FILE-NORMALE  
      

Anche i seguenti valori di attributo di file attivano la replica, anche se non possono `SetFileAttributes` essere impostati tramite la `GetFileAttributes` funzione (usare la funzione per visualizzare i valori dell'attributo).

  - REPARSE\_\_POINT\_DELL'ATTRIBUTO FILE  
      

> [!NOTE]
> Replica DFS non replica i valori dell'attributo reparse point, a meno che il tag reparse non sia IO_REPARSE_TAG_SYMLINK. I file con i tag IO_REPARSE_TAG_DEDUP, IO_REPARSE_TAG_SIS o IO_REPARSE_TAG_HSM reparse vengono replicati come file normali. Tuttavia, il tag di reparse e i buffer dei dati di analisi non vengono replicati in altri server perché il reparse point funziona solo sul sistema locale. 
<br>

  - \_ATTRIBUTO\_FILE COMPRESSO  
      
  - \_ATTRIBUTO\_FILE CRITTOGRAFATO  
      

> [!NOTE]
> Replica DFS non replica i file crittografati tramite Encrypting File System (EFS). Replica DFS replica i file crittografati utilizzando un software non Microsoft, ma solo se non imposta il valore dell'attributo FILE_ATTRIBUTE_ENCRYPTED per il file. 
<br>

  - FILE\_\_SPARSE\_ATTRIBUTO FILE  
      
  - DIRECTORY\_DEGLI\_ATTRIBUTI DI FILE  
      

Replica DFS non replica il valore temporaneo\_dell'\_attributo file.

### <a name="can-i-control-which-member-is-replicated"></a>È possibile controllare quale membro è replicato?

Sì. È possibile scegliere una topologia quando si crea un gruppo di replica. In alternativa, è possibile selezionare **Nessuna topologia** e configurare manualmente le connessioni dopo che è stato creato il gruppo di replica.

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>È possibile eseguire il seeding di un membro del gruppo di replica con dati prima della replica iniziale?

Sì. Replica DFS supporta la copia dei file in un membro del gruppo di replica prima della replica iniziale. Questa "pre-staging" può ridurre notevolmente la quantità di dati replicati durante la replica iniziale.

La replica iniziale non deve replicare il contenuto quando i file differiscono solo per attributi reali o timestamp. Un attributo reale è un attributo che può essere impostato tramite la funzione `SetFileAttributes`Win32. Per ulteriori informazioni, vedere la [funzione SetFileAttributes](http://go.microsoft.com/fwlink/?linkid=182269) in MSDN Library (http://go.microsoft.com/fwlink/?LinkId=182269). Se due file sono diversi da altri attributi, ad esempio la compressione, il contenuto del file viene replicato.

Per pre-installare un membro del gruppo di replica, copiare i file nella cartella appropriata nei server di destinazione, creare il gruppo di replica e quindi scegliere un membro primario. Scegliere il membro con i file più aggiornati da replicare perché il contenuto del membro primario è considerato "autorevole". Ciò significa che durante la replica iniziale, i file del membro primario sovrascriveranno sempre le altre versioni dei file su altri membri del gruppo di replica.

Per informazioni sul pre-seeding e sulla clonazione del database DFSR, vedere [replica DFS sincronizzazione iniziale in Windows Server 2012 R2: Attacco dei cloni](http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx).

Per altre informazioni sulla replica iniziale, vedere [creare un gruppo di replica](https://technet.microsoft.com/library/cc725893).

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>Replica DFS superare i problemi comuni del servizio Replica file?

Sì. Replica DFS si verificano tre problemi di FRS comuni:

  - Wrapping del journal: Replica DFS esegue il ripristino dal diario in tempo reale. Ogni file o cartella esistente verrà contrassegnata come journalWrap ed è stata verificata in base al file system prima che la replica sia nuovamente abilitata. Durante il ripristino, questo volume non è disponibile per la replica in entrambe le direzioni.  
      
  - Replica eccessiva: Per evitare una replica eccessiva, Replica DFS usa un sistema di crediti.  
      
  - Cartelle morphed: Per evitare i nomi di cartella morphed, replica DFS archivia i dati in conflitto in\\una cartella DfsrPrivate cartella ConflictAndDeleted nascosta, che si trova nel percorso locale della cartella replicata. Ad esempio, la creazione di più cartelle simultaneamente con nomi identici su server diversi replicati con FRS causa la ridenominazione della cartella o delle cartelle precedenti da parte di FRS. Replica DFS sposta invece le cartelle precedenti nel conflitto locale e nella cartella eliminata.  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>Replica DFS replicare i file in ordine cronologico?

No. I file possono essere replicati in modo non corretto.

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>Replica DFS replicare i file utilizzati da un'altra applicazione?

Se un'applicazione apre un file e crea un blocco di file su di esso (impedendone l'utilizzo da parte di altre applicazioni mentre è aperto), Replica DFS non eseguirà la replica del file fino alla chiusura. Se l'applicazione apre il file con accesso in lettura e condivisione, il file può comunque essere replicato.

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>Replica DFS replicare le autorizzazioni per i file NTFS, i flussi di dati alternativi, i collegamenti reali e i reparse point?

  - Replica DFS replica le autorizzazioni per i file NTFS e i flussi di dati alternativi.  
      
  - Microsoft non supporta la creazione di collegamenti reali NTFS a o da file in una cartella replicata. questa operazione può causare problemi di replica con i file interessati. I file dei collegamenti reali vengono ignorati da Replica DFS e non vengono replicati. Anche i punti di giunzione non vengono replicati e Replica DFS registra l'evento 4406 per ogni punto di giunzione rilevato.  
      
  - Gli unici reparse point replicati da replica DFS sono quelli che usano il tag\_del\_collegamento\_simbolico di reparse i/o. Tuttavia, replica DFS non garantisce che venga replicata anche la destinazione di un collegamento simbolico. Per ulteriori informazioni, vedere il [Blog del team di Ask the Directory Services](http://blogs.technet.com/b/askds/archive/2011/09/30/friday-mail-sack-super-slo-mo-edition.aspx).  
      
  - File con\_i/o reparse\_\_Tag deduplicazione,\_i/\_o\_reparse Tag SIS\_o io\_reparse\_Tag HSM reparse tag vengono replicati come file normali. I buffer di reparse tag e reparse data non vengono replicati in altri server perché il reparse point funziona solo sul sistema locale. Di conseguenza, Replica DFS possibile replicare le cartelle in volumi che usano la deduplicazione dati in Windows Server 2012 o Single Instance Storage (SIS), tuttavia, le informazioni di deduplicazione dati vengono gestite separatamente da ogni server in cui è abilitato il servizio ruolo.  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>Replica DFS replicare le modifiche del timestamp se non vengono apportate altre modifiche al file?

No, Replica DFS non replica i file per i quali l'unica modifica è una modifica apportata al timestamp. Il timestamp modificato, inoltre, non viene replicato in altri membri del gruppo di replica, a meno che non vengano apportate altre modifiche al file.

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>Replica DFS replicare le autorizzazioni aggiornate per un file o una cartella?

Sì. Replica DFS replica le modifiche delle autorizzazioni per file e cartelle. Solo la parte del file associata all'elenco di controllo di accesso (ACL) viene replicata, sebbene Replica DFS debba comunque leggere l'intero file nell'area di gestione temporanea.


> [!NOTE]
> La modifica degli ACL in un numero elevato di file può influito sulle prestazioni della replica. Tuttavia, quando si utilizza RDC, la quantità di dati trasferiti è proporzionale alle dimensioni degli ACL, non alla dimensione dell'intero file. La quantità di traffico su disco è ancora proporzionale alle dimensioni dei file perché i file devono essere letti da e verso la cartella di gestione temporanea. 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>Replica DFS supporta l'Unione di file di testo in caso di conflitto?

Replica DFS non unisce i file quando si verifica un conflitto. Tuttavia, tenta di mantenere la versione precedente del file nella cartella DfsrPrivate\\cartella ConflictAndDeleted nascosta nel computer in cui è stato rilevato il conflitto.

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>Replica DFS utilizza la crittografia durante la trasmissione dei dati?

Sì. Replica DFS utilizza le connessioni RPC (Remote Procedure Call) con la crittografia.

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>È possibile disabilitare l'utilizzo di RPC crittografate?

No. Il servizio Replica DFS utilizza RPC (Remote Procedure Call) su TCP per replicare i dati. Per proteggere i trasferimenti di dati su Internet, il servizio Replica DFS è progettato in modo da usare sempre la costante `RPC_C_AUTHN_LEVEL_PKT_PRIVACY`a livello di autenticazione. In questo modo si garantisce che la comunicazione RPC in Internet sia sempre crittografata. Pertanto, non è possibile disabilitare l'utilizzo di RPC crittografate da parte del servizio Replica DFS.

Per ulteriori informazioni, vedere i siti Web Microsoft seguenti:

  - [Riferimento tecnico RPC](http://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [Informazioni sulla compressione differenziale remota](http://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [Costanti a livello di autenticazione](http://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>Come vengono gestite le repliche simultanee?

Esiste una gestione aggiornamenti per ogni cartella replicata. I gestori degli aggiornamenti funzionano indipendentemente l'uno dall'altro.

Per impostazione predefinita, un massimo di 16 (quattro in Windows Server 2003 R2) download simultanei vengono condivisi tra tutte le connessioni e i gruppi di replica. Poiché le connessioni e gli aggiornamenti del gruppo di replica non vengono serializzati, non esiste un ordine specifico in cui vengono ricevuti gli aggiornamenti. Se vengono aperte due pianificazioni, gli aggiornamenti vengono in genere ricevuti e installati da entrambe le connessioni nello stesso momento.

### <a name="how-do-i-force-replication-or-polling"></a>Ricerca per categorie forzare la replica o il polling?

È possibile forzare immediatamente la replica usando Gestione DFS, come descritto in [modificare le pianificazioni della replica](https://technet.microsoft.com/library/Cc732278). È anche possibile forzare la replica usando `Sync-DfsReplicationGroup` il cmdlet, incluso nel modulo DFSR di PowerShell introdotto con Windows Server 2012 R2 oppure il comando **Dfsrdiag SyncNow** . È possibile forzare il polling usando `Update-DfsrConfigurationFromAD` il cmdlet o il comando **Dfsrdiag PollAD** .

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>È possibile configurare un tempo di attesa tra le repliche per i file che cambiano di frequente?

No. Se la pianificazione è aperta, Replica DFS replica le modifiche non appena vengono note. Non è possibile configurare un'ora non interattiva per i file.

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>È possibile configurare la replica unidirezionale con Replica DFS?

Sì. Se si usa Windows Server 2012 o Windows Server 2008 R2, è possibile creare una cartella replicata di sola lettura che replica il contenuto tramite una connessione unidirezionale. Per ulteriori informazioni, vedere [rendere una cartella replicata di sola lettura su un particolare membro](http://go.microsoft.com/fwlink/?linkid=156740) (http://go.microsoft.com/fwlink/?LinkId=156740).

Non è supportata la creazione di una connessione di replica unidirezionale con Replica DFS in Windows Server 2008 o Windows Server 2003 R2. Questa operazione può causare numerosi problemi, ad esempio errori di topologia del controllo di integrità, problemi di gestione temporanea e problemi con il database di Replica DFS.

Se si usa Windows Server 2008 o Windows Server 2003 R2, è possibile simulare una connessione unidirezionale eseguendo le azioni seguenti:

  - Eseguire il training degli amministratori per apportare modifiche solo ai server che si desidera designare come server primari. Quindi, consentire la replica delle modifiche nei server di destinazione.  
      
  - Configurare le autorizzazioni di condivisione nei server di destinazione in modo che gli utenti finali non dispongano delle autorizzazioni di scrittura. Se non sono consentite modifiche nei server di succursale, non è necessario eseguire alcuna replica, simulando una connessione unidirezionale e mantenendo basso l'utilizzo della rete WAN.  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>Esiste un modo per forzare una replica completa di tutti i file, inclusi i file non modificati?

No. Se Replica DFS considera i file identici, non li eseguirà la replica. Se i file modificati non sono stati replicati, Replica DFS li replica automaticamente quando vengono configurati. Per sovrascrivere la pianificazione configurata, utilizzare il metodo WMI **ForceReplicate ()** . Tuttavia, si tratta solo di un override della pianificazione e non forza la replica di file non modificati o identici.

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>Cosa accade se il membro primario soffre di una perdita di database durante la replica iniziale?

Durante la replica iniziale, i file del membro primario avranno sempre la precedenza nella risoluzione dei conflitti che si verifica se i membri riceventi hanno versioni diverse dei file nel membro primario. La designazione del membro primario è archiviata in Active Directory Domain Services e la designazione viene cancellata dopo che il membro primario è pronto per la replica, ma prima della replica di tutti i membri del gruppo di replica.

Se la replica iniziale ha esito negativo o il servizio Replica DFS viene riavviato durante la replica, il membro primario Visualizza la designazione del membro primario nel database di Replica DFS locale e ritenta la replica iniziale. Se il database di Replica DFS del membro primario viene perso dopo la cancellazione della designazione primaria in Active Directory Domain Services, ma prima che tutti i membri del gruppo di replica completino la replica iniziale, tutti i membri del gruppo di replica non riescono a replicare la cartella perché nessun server è designato come membro primario. In tal caso, usare il comando **DfsrAdmin Membership/Set/IsPrimary: true** sul server membro primario per ripristinare manualmente la designazione del membro primario.

Per altre informazioni sulla replica iniziale, vedere [creare un gruppo di replica](https://technet.microsoft.com/library/cc725893).


> [!WARNING]
> La designazione del membro primario viene utilizzata solo durante il processo di replica iniziale. Se si usa il comando <STRONG>DfsrAdmin</STRONG> per specificare un membro primario per una cartella replicata al termine della replica, replica DFS non designa il server come membro primario nel Active Directory Domain Services. Tuttavia, se il database di Replica DFS sul server subisce un danneggiamento irreversibile o una perdita di dati, il server tenta di eseguire una replica iniziale come membro primario anziché recuperare i dati da un altro membro della replica gruppo. In sostanza, il server diventa un server primario non autorizzato, che può causare conflitti. Per questo motivo, specificare manualmente il membro primario solo se si è certi che la replica iniziale non è riuscita. 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>Cosa accade se la pianificazione della replica viene chiusa durante la replica di un file?

Se per la connessione è abilitata la compressione differenziale remota (RDC), la replica in ingresso di un file di dimensioni superiori a 64 KB che ha iniziato la replica immediatamente prima della chiusura della pianificazione (o la modifica a **nessuna larghezza di banda**) continua quando viene aperta la pianificazione (o modifiche a un valore diverso da **nessuna larghezza di banda**). La replica continua dallo stato in cui si trovava quando la replica è stata arrestata.

Se RDC è disattivato, Replica DFS riavvia completamente il trasferimento di file. Questo può ritardare quando il file è disponibile nel membro ricevente.

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>Cosa accade quando due utenti aggiornano contemporaneamente lo stesso file su server diversi?

Quando Replica DFS rileva un conflitto, viene usata la versione del file salvata per ultima. Sposta l'altro file nella cartella DfsrPrivate\\cartella ConflictAndDeleted (nel percorso locale della cartella replicata nel computer che ha risolto il conflitto). Rimane disponibile fino a quando la pulizia delle cartelle in conflitto e con eliminazione non viene completata, che si verifica quando la cartella dei conflitti e degli eliminati supera le dimensioni configurate o Replica DFS rileva un errore di spazio su disco insufficiente. La cartella dei conflitti e degli eliminati non viene replicata e questo metodo di risoluzione dei conflitti evita il problema delle directory morphed possibili in FRS.

Quando si verifica un conflitto, Replica DFS registra un evento informativo nel registro eventi Replica DFS. Questo evento non richiede l'intervento dell'utente per i motivi seguenti:

  - Non è visibile agli utenti (è visibile solo agli amministratori del server).  
      
  - Replica DFS considera la cartella dei conflitti e degli eliminati come una cache. Quando viene raggiunta una soglia di quota, questi file vengono eliminati. Non vi è alcuna garanzia che i file in conflitto vengano salvati.  
      
  - Il conflitto potrebbe trovarsi in un server diverso dall'origine del conflitto.  
      

## <a name="staging"></a>Gestione temporanea

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>Replica DFS continuare a eseguire la gestione temporanea quando la replica è disabilitata con una quota di limitazione della larghezza di banda o una pianificazione o quando una connessione viene disabilitata manualmente?

No. Replica DFS non continua a organizzare i file al di fuori degli orari di replica pianificati, se è stata superata la quota di limitazione della larghezza di banda o quando le connessioni sono disabilitate.

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>Replica DFS impedisce ad altre applicazioni di accedere a un file durante la gestione temporanea?

No. Replica DFS apre i file in modo da impedire agli utenti o alle applicazioni di aprire i file nella cartella replica. Questo metodo è noto come "blocco opportunistico".

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>È possibile modificare il percorso della cartella di gestione temporanea con lo strumento di gestione DFS?

Sì. Il percorso della cartella di gestione temporanea viene configurato nella scheda **Avanzate** della finestra di dialogo **Proprietà** per ogni membro di un gruppo di replica.

### <a name="when-are-files-staged"></a>Quando vengono gestiti i file?

I file vengono gestiti temporaneamente sul membro mittente quando il membro ricevente richiede il file, a meno che il file non sia 64 KB o inferiore, come illustrato nella tabella seguente. Se la compressione differenziale remota (RDC) è disabilitata sulla connessione, il file viene gestito temporaneamente a meno che non sia 256 KB o inferiore. I file vengono anche gestiti in modo temporaneo nel membro ricevente mentre vengono trasferiti se hanno dimensioni inferiori a 64 KB, sebbene sia possibile configurare questa impostazione tra 16 KB e 1 MB. Se la pianificazione è chiusa, i file non vengono gestiti temporaneamente.

### <a name="the-minimum-file-sizes-for-staging-files"></a>Dimensioni minime dei file per i file di staging

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>RDC abilitato</th>
<th>RDC disabilitato</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>Membro di invio</p></td>
<td><p>64 KB</p></td>
<td><p>256 KB</p></td>
</tr>
<tr class="odd">
<td><p>Membro di ricezione</p></td>
<td><p>64 KB per impostazione predefinita</p></td>
<td><p>64 KB per impostazione predefinita</p></td>
</tr>
</tbody>
</table>

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>Cosa accade se un file viene modificato dopo essere stato sottoposto a staging ma prima che venga completamente trasmesso al sito remoto?

Se una parte del file è già in fase di trasmissione, Replica DFS continua la trasmissione. Se il file viene modificato prima che Replica DFS inizi a trasmettere il file, viene inviata la versione più recente del file.

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
<td><p>Aggiornamento per Windows Server 2019.</p></td>
<td><p>Nuovo sistema operativo.</p></td>
</tr>
<tr class="even">
<td><p>9 ottobre 2013</p></td>
<td><p>Sono stati aggiornati i limiti di Replica DFS supportati? sezione con risultati dei test in Windows Server 2012 R2.</p></td>
<td><p>Aggiornamenti per la versione più recente di Windows Server</p></td>
</tr>
<tr class="odd">
<td><p>30 gennaio 2013</p></td>
<td><p>È stato aggiunto il Replica DFS continuare a eseguire i file di staging quando la replica è disabilitata con una quota di limitazione della larghezza di banda o una pianificazione o quando una connessione viene disabilitata manualmente? voce.</p></td>
<td><p>Domande dei clienti</p></td>
</tr>
<tr class="even">
<td><p>31 ottobre 2012</p></td>
<td><p>Modificare i limiti di Replica DFS supportati? voce per aumentare il numero testato di file replicati in un volume.</p></td>
<td><p>Feedback dei clienti</p></td>
</tr>
<tr class="odd">
<td><p>15 agosto 2012</p></td>
<td><p>Modificato il Replica DFS replicare le autorizzazioni per i file NTFS, i flussi di dati alternativi, i collegamenti reali e i reparse point? voce per chiarire ulteriormente il modo in cui Replica DFS gestisce i collegamenti reali e i reparse point.</p></td>
<td><p>Commenti e suggerimenti del servizio supporto tecnico clienti</p></td>
</tr>
<tr class="even">
<td><p>13 giugno, 2012</p></td>
<td><p>Modificato il Replica DFS funziona sui volumi ReFS o FAT? voce per aggiungere una discussione su ReFS.</p></td>
<td><p>Feedback dei clienti</p></td>
</tr>
<tr class="odd">
<td><p>25 aprile 2012</p></td>
<td><p>Modificato il Replica DFS replicare le autorizzazioni per i file NTFS, i flussi di dati alternativi, i collegamenti reali e i reparse point? per chiarire il modo in cui Replica DFS gestisce i collegamenti reali.</p></td>
<td><p>Riduzione della confusione potenziale</p></td>
</tr>
<tr class="even">
<td><p>30 marzo 2011</p></td>
<td><p>È stato modificato il Replica DFS possibile replicare i file di database di Outlook. pst o Microsoft Office Access? voce per correggere il potenziale impatto dell'utilizzo di Replica DFS con i file con estensione pst e Access.</p>
<p>Aggiunta di come è possibile migliorare le prestazioni della replica?</p></td>
<td><p>Domande del cliente sulla voce precedente, che indica erroneamente che la replica di file con estensione pst o Access potrebbe danneggiare il database Replica DFS.</p></td>
</tr>
<tr class="odd">
<td><p>26 gennaio 2011</p></td>
<td><p>È stato aggiunto come è possibile ripristinare i file dalle cartelle cartella ConflictAndDeleted o preesistenti?</p></td>
<td><p>Feedback dei clienti</p></td>
</tr>
<tr class="even">
<td><p>20 ottobre 2010</p></td>
<td><p>È stato aggiunto come è possibile aggiornare o sostituire un membro Replica DFS?</p></td>
<td><p>Feedback dei clienti</p></td>
</tr>
</tbody>
</table>

