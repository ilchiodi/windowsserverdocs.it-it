---
title: 'Replica DFS: Domande frequenti'
ms.date: 06/18/2014
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 1e11f6c596d7e5eb0bdf379adcf47d21e74e9f6b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815624"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>Replica DFS: Domande frequenti


Aggiornamento: 30 aprile 2019

Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Questo articolo contiene le risposte alle domande frequenti sul servizio Replica DFS (Distributed File System), noto anche come DFS-R o DFSR, per Windows Server.

Per informazioni su Spazi dei nomi DFS, vedere il documento di [domande frequenti su Spazi dei nomi DFS](https://technet.microsoft.com/library/ee404780).

Per informazioni sulle novità di Replica DFS, vedi gli argomenti seguenti:

  - [Panoramica di Spazi dei nomi DFS e Replica DFS](https://technet.microsoft.com/library/jj127250) (in Windows Server 2012)  
      
  - [Novità del file system distribuito](https://technet.microsoft.com/library/ee307957) in [Modifiche apportate alle funzionalità da Windows Server 2008 a Windows Server 2008 R2](https://technet.microsoft.com/library/dd391932)  
      
  - [File system distribuito](https://technet.microsoft.com/library/cc753479) in [Modifiche apportate alle funzionalità da Windows Server 2003 con SP1 a Windows Server 2008](https://technet.microsoft.com/library/cc753208)  
      

Per ottenere un elenco delle modifiche apportate di recente a questo argomento, vedere la sezione [Cronologia delle modifiche](#change-history) di questo argomento.

      

## <a name="interoperability"></a>Interoperabilità

### <a name="can-dfs-replication-communicate-with-frs"></a>Replica DFS può comunicare con il servizio Replica file?

No. Replica DFS non comunica con il servizio Replica file (FRS, File Replication Service). Replica DFS e il servizio Replica file possono essere eseguiti contemporaneamente nello stesso server, ma non devono mai essere configurati per la replica delle stesse cartelle o sottocartelle perché questo può causare la perdita di dati.

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>Replica DFS può sostituire il servizio Replica file per la replica di SYSVOL?

Sì. Replica DFS può sostituire il servizio Replica file per la replica di SYSVOL sui server che eseguono Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. I server con Windows Server 2003 R2 non consentono l'uso di Replica DFS per la replica della cartella SYSVOL.

Per altre informazioni sulla replica di SYSVOL con Replica DFS, vedi [Guida alla migrazione della replica di SYSVOL dal servizio Replica file a Replica DFS](https://technet.microsoft.com/library/dd640019).

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>Posso eseguire l'aggiornamento dal servizio Replica file a Replica DFS senza perdere le impostazioni di configurazione?

Sì. Per eseguire la migrazione della replica dal servizio Replica file a Replica DFS, vedi i documenti seguenti:

  - Per eseguire la migrazione della replica di cartelle diverse da SYSVOL, vedi [Guida operativa di DFS: migrazione dal servizio Replica file a Replica DFS](https://go.microsoft.com/fwlink/?linkid=192776) e [FRS2DFSR: un'utilità per la migrazione dal servizio Replica file a Replica DFS](https://go.microsoft.com/fwlink/?linkid=195437) (https://go.microsoft.com/fwlink/?LinkID=195437).  
      
  - Per eseguire la migrazione della replica della cartella SYSVOL a Replica DFS, vedi [Guida alla migrazione della replica di SYSVOL: dal servizio Replica file a Replica DFS](https://technet.microsoft.com/library/dd640019).  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>Posso usare Replica DFS in un ambiente misto Windows/UNIX?

Sì. Anche se Replica DFS supporta solo la replica di contenuto tra server che eseguono Windows Server, i client UNIX possono accedere alle condivisioni file nei server Windows. A tale scopo, installa i servizi per NFS (Network File System) nel server di Replica DFS.

Puoi anche usare la funzionalità client SMB/CIFS inclusa in molti client UNIX per accedere direttamente alle condivisioni file di Windows, anche se questa funzionalità è spesso limitata o richiede modifiche all'ambiente Windows, ad esempio la disabilitazione della firma SMB tramite Criteri di gruppo.

Replica DFS interagisce con NFS in un server che esegue un sistema operativo Windows Server, ma non puoi replicare un punto di montaggio NFS.

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>Posso usare il Servizio Copia Shadow del volume con Replica DFS?

Sì. Il servizio Replica DFS è supportato nei volumi del Servizio Copia Shadow del volume (VSS, Volume Shadow Copy Service) e gli snapshot precedenti possono essere ripristinati con il Client versioni precedenti.

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>Posso usare Windows Backup (Ntbackup.exe) per eseguire il backup remoto di una cartella replicata?

No. L'uso di Windows Backup (Ntbackup.exe) in un computer con Windows Server 2003 o versioni precedenti per il backup del contenuto di una cartella replicata in un computer con Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 non è supportato.

Per eseguire il backup dei file archiviati in una cartella replicata, usa Windows Server Backup o Microsoft&reg; System Center Data Protection Manager. Per informazioni sulla funzionalità Backup e ripristino in Windows Server 2008 R2 e Windows Server 2008, vedi [Backup e ripristino](https://technet.microsoft.com/library/Cc754097). Per altre informazioni, vedi [System Center Data Protection Manager](https://go.microsoft.com/fwlink/?linkid=182261) (https://go.microsoft.com/fwlink/?LinkId=182261).

### <a name="do-file-system-policies-impact-dfs-replication"></a>I criteri del file system influiscono su Replica DFS?

Sì. Non configurare criteri del file system sulle cartelle replicate. Questi criteri riapplicano le autorizzazioni NTFS a ogni intervallo di aggiornamento di Criteri di gruppo. Questo può comportare violazioni della condivisione perché un file aperto non viene replicato fino alla chiusura.

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>Replica DFS esegue la replica delle cassette postali ospitate in Microsoft Exchange Server?

No. Il servizio Replica DFS non può essere usato per replicare le cassette postali ospitate su Microsoft Exchange Server.

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>Replica DFS supporta gli screening dei file creati da Gestione risorse file server?

Sì. Tuttavia, le impostazioni relative agli screening dei file di Gestione risorse file server (FSRM, File Server Resource Manager) devono corrispondere a entrambe le estremità della replica. Replica DFS dispone inoltre di uno specifico meccanismo di filtro per i file e le cartelle che puoi usare per escludere dalla replica determinati file e tipi di file.

Di seguito sono riportate le procedure consigliate per l'implementazione di quote o screening dei file:

  - La cartella nascosta DfsrPrivate non deve essere soggetta a quote o screening dei file.  
      
  - Nelle cartelle replicate non devono essere presenti file soggetti a screening prima che lo screening venga abilitato.  
      
  - Nessuna cartella può superare la quota prima che la quota venga abilitata.  
      
  - Le quote rigide devono essere usate con cautela. Può accadere che singoli membri di un gruppo di replica rimangano entro i limiti di una quota prima della replica, ma che la superino quando i file vengono replicati. Se, ad esempio, un utente copia un file da 10 megabyte (MB) nel server A (che quindi raggiunge il limite rigido) e un altro utente copia un file da 5 MB nel server B, quando viene eseguita la replica successiva, entrambi i server supereranno la quota di 5 MB. In una situazione di questo tipo è possibile che il servizio Replica DFS provi continuamente a replicare i file, causando buchi nel vettore di versione e possibili problemi di prestazioni.  
      

### <a name="is-dfs-replication-cluster-aware"></a>Replica DFS è in grado di riconoscere i cluster?

Sì. Replica DFS in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2 include la capacità di aggiungere un cluster di failover come membro di un gruppo di replica. Per altre informazioni, vedi [Aggiungere un membro a un gruppo di replica](https://go.microsoft.com/fwlink/?linkid=155085) (https://go.microsoft.com/fwlink/?LinkId=155085). Nelle versioni di Windows precedenti a Windows Server 2008 R2 il servizio Replica DFS non prevede il coordinamento con un cluster di failover, pertanto il failover su un altro nodo non è supportato.


> [!NOTE]
> Replica DFS non supporta inoltre la replica di file nei volumi condivisi del cluster. 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>Replica DFS è compatibile con Deduplicazione dati?

Sì. Replica DFS può replicare le cartelle in volumi che usano Deduplicazione dati in Windows Server.

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>Replica DFS è compatibile con RIS e WDS?

Sì. Replica DFS replica i volumi in cui è abilitata la funzionalità Single Instance Storage (SIS). SIS viene usata da Servizi di installazione remota (RIS, Remote Installation Services), Servizi di distribuzione Windows (WDS, Windows Deployment Services) e Windows Storage Server.

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>È possibile usare Replica DFS con File offline?

Puoi usare contemporaneamente Replica DFS e File offline senza alcun problema in scenari in cui è presente un solo utente alla volta che scrive nei file. Questa operazione è utile per gli utenti che si spostano tra due sedi e vogliono poter accedere ai file in ogni sede o in modalità offline. File offline memorizza i file nella cache locale per l'uso in modalità offline e Replica DFS replica i dati tra le due sedi.

Non usare Replica DFS insieme a File offline in un ambiente multiutente perché Replica DFS non offre meccanismi di blocco distribuito o funzionalità di estrazione dei file. Se due utenti modificano lo stesso file contemporaneamente in server diversi, Replica DFS sposta il file precedente nella cartella DfsrPrivate\\ConflictandDeleted (nel percorso locale della cartella replicata) durante la replica successiva.

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>Quali applicazioni antivirus sono compatibili con Replica DFS?

Le applicazioni antivirus possono causare un numero eccessivo di processi di replica se le attività di scansione modificano i file in una cartella replicata. Per altre informazioni, vedi [Test dell'interoperabilità delle applicazioni antivirus con Replica DFS](https://go.microsoft.com/fwlink/?linkid=73990) (https://go.microsoft.com/fwlink/?LinkId=73990).

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>Quali vantaggi offre l'uso di Replica DFS in sostituzione di Windows SharePoint Services?

Windows&reg; SharePoint&reg; Services assicura una coerenza rigorosa in termini di estrazione dei file che il servizio Replica DFS non è in grado di offrire. Se nel tuo ambiente è importante che più persone possano modificare lo stesso file, ti consigliamo di usare Windows SharePoint Services. Windows SharePoint Services 2.0 con Service Pack 2 è disponibile come componente di Windows Server 2003 R2. Windows SharePoint Services può essere scaricato dal sito Web Microsoft, ma non è incluso nelle versioni più recenti di Windows Server. Se tuttavia esegui la replica dei dati tra più siti e prevedi che gli utenti non debbano modificare contemporaneamente gli stessi file, Replica DFS garantisce una maggiore larghezza di banda e una gestione più semplice.

## <a name="limitations-and-requirements"></a>Limitazioni e requisiti

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>Replica DFS può eseguire la replica tra diverse sedi senza una connessione VPN?

Sì, a condizione che sia presente una connessione WAN (Wide Area Network) privata (non Internet) tra le sedi. Devi tuttavia aprire le porte appropriate nei firewall esterni. Replica DFS usa l'Agente mapping endpoint RPC (porta 135) e una porta temporanea assegnata in modo casuale con un numero superiore a 1024. Puoi usare lo strumento da riga di comando **Dfsrdiag** per specificare una porta statica in sostituzione della porta temporanea. Per altre informazioni su come specificare l'Agente mapping endpoint RPC, vedi l'[articolo 154596](https://go.microsoft.com/fwlink/?linkid=73991) della Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=73991).

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>Replica DFS può replicare i file crittografati con Encrypting File System?

No. Replica DFS non esegue la replica di cartelle o file crittografati con Encrypting File System (EFS). Se un utente esegue la crittografia di un file replicato in precedenza, Replica DFS elimina il file da tutti gli altri membri del gruppo di replica. Questo consente di assicurarsi che l'unica copia disponibile del file sia la versione crittografata sul server.

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>Replica DFS può replicare i file di database di Microsoft Office Access o i file di Outlook con estensione pst?

Replica DFS può eseguire in modo sicuro la replica dei file delle cartelle personali di Microsoft Outlook (.pst) e dei file di Microsoft Access solo se sono stati effettivamente archiviati e non sono accessibili attraverso la rete tramite un client come Outlook o Access (prima di aprire i file di Access o quelli con estensione pst, copiali in un dispositivo di archiviazione locale). I motivi sono i seguenti:

  - L'apertura dei file con estensione pst attraverso connessioni di rete può causare il danneggiamento dei dati presenti in tali file. Per altre informazioni sul motivo per cui non è possibile accedere in modo sicuro ai file con estensione pst attraverso una rete, vedi l'[articolo 297019](https://go.microsoft.com/fwlink/?linkid=125363) della Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=125363).  
      
  - I file di Access e quelli con estensione pst tendono a rimanere aperti a lungo quando un client come Outlook o Office Access accede a tali file. Questo impedisce a Replica DFS di replicare i file finché non vengono chiusi.  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>Posso usare Replica DFS in un gruppo di lavoro?

No. Replica DFS si basa su Active Directory&reg; Domain Services per la configurazione. Funziona solo all'interno di un dominio.

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>È possibile replicare più cartelle in un singolo server?

Sì. Replica DFS può replicare numerose cartelle tra i server. Verifica che ogni cartella replicata disponga di un percorso radice univoco e che non vi siano sovrapposizioni. Ad esempio, D:\\Vendite e D:\\Gestione possono essere i percorsi radice di due cartelle replicate, mentre D:\\Vendite e D:\\Vendite\\Rapporti non sono consentiti.

### <a name="does-dfs-replication-require-dfs-namespaces"></a>Replica DFS richiede l'uso di Spazi dei nomi DFS?

No. Replica DFS e Spazi dei nomi DFS possono essere usati insieme o separatamente. Il servizio Replica DFS può inoltre essere usato per replicare spazi dei nomi DFS autonomi, mentre questo non è possibile con il servizio Replica file.

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>Replica DFS richiede la sincronizzazione dell'ora tra i server?

No. Replica DFS non richiede esplicitamente la sincronizzazione dell'ora tra i server, ma gli orologi dei server devono comunque corrispondere con una certa precisione. Per il corretto funzionamento dell'autenticazione Kerberos, gli orologi dei server devono essere impostati con una differenza massima di cinque minuti (per impostazione predefinita). Replica DFS, ad esempio, usa i timestamp per determinare quale file ha la precedenza in caso di conflitto. La precisione dell'ora è importante anche per la Garbage Collection, le pianificazioni e altre funzionalità.

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>Replica DFS supporta la replica di un intero volume?

Sì. È tuttavia necessario installare prima Windows Server 2003 Service Pack 2 o l'hotfix. Per altre informazioni, vedi l'[articolo 920335](https://go.microsoft.com/fwlink/?linkid=76776) della Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=76776). La replica di un intero volume può inoltre causare i problemi seguenti:

  - Se il volume contiene un file di paging di Windows, la replica ha esito negativo e riporta l'evento DFSR 4312 nel registro eventi di sistema.  
      
  - Replica DFS imposta gli attributi di sistema e nascosti nella cartella replicata nei server di destinazione. Questo avviene perché, per impostazione predefinita, Windows applica tali attributi alla cartella radice del volume. Se il percorso locale della cartella replicata nei server di destinazione è anche una radice del volume, non vengono apportate altre modifiche agli attributi della cartella.  
      
  - Durante la replica di un volume che contiene la cartella di sistema di Windows, Replica DFS riconosce la cartella %WINDIR% e non ne esegue replica. Replica DFS esegue tuttavia la replica delle cartelle usate dalle applicazioni non Microsoft e questo può causare errori nelle applicazioni sui server di destinazione se queste presentano problemi di interoperabilità con Replica DFS.  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>Replica DFS supporta RPC su HTTP?

No.

### <a name="does-dfs-replication-work-across-wireless-networks"></a>Replica DFS funziona attraverso le reti wireless?

Sì. Replica DFS è indipendente dal tipo di connessione.

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>Replica DFS funziona sui volumi ReFS o FAT?

No. Replica DFS supporta solo volumi formattati con il file system NTFS. I file system ReFS (Resilient File System) e FAT non sono supportati. Replica DFS richiede NTFS perché usa il journal delle modifiche di NTFS e altre funzionalità di questo file system.

### <a name="does-dfs-replication-work-with-sparse-files"></a>Replica DFS funziona con i file sparse?

Sì. Puoi eseguire la replica dei file sparse. L'attributo **Sparse** viene mantenuto nel membro di destinazione.

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>Devo accedere come amministratore per eseguire la replica dei file?

No. Replica DFS è un servizio eseguito con l'account di sistema locale, pertanto non devi accedere come amministratore per eseguire la replica. Devi tuttavia essere un amministratore di dominio o locale dei file server interessati se vuoi apportare modifiche alla configurazione di Replica DFS.

Per altre informazioni, vedi "Delega e requisiti di sicurezza di Replica DFS" in [Delegare la possibilità di gestire Replica DFS](https://go.microsoft.com/fwlink/?linkid=182294) (https://go.microsoft.com/fwlink/?LinkId=182294).

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>Come posso aggiornare o sostituire un membro di Replica DFS?

Per aggiornare o sostituire un membro di Replica DFS, vedi questo post del blog del team di Directory Services: [Replacing DFSR Member Hardware or OS](https://blogs.technet.com/b/askds/archive/2010/09/10/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os.aspx) (Sostituzione dell'hardware o del sistema operativo di un membro di DFSR).

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>Replica DFS è adatto alla replica di profili mobili?

Sì. Alcuni scenari di replica dei profili utente mobili sono supportati. Per informazioni sugli scenari supportati, vedi l'[informativa del supporto tecnico Microsoft sui dati dei profili utente replicati](https://go.microsoft.com/fwlink/?linkid=201282) (https://go.microsoft.com/fwlink/?LinkId=201282).

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>È previsto un limite di caratteri per i file o un limite di profondità per le cartelle?

Windows e Replica DFS supportano percorsi di cartelle con un massimo di 32.000 caratteri. In Replica DFS i percorsi di cartella non sono limitati a 260 caratteri.

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>I membri di un gruppo di replica devono trovarsi nello stesso dominio?

No. I gruppi di replica possono estendersi tra più domini all'interno di una stessa foresta ma non tra foreste diverse.

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>Quali sono i limiti supportati per Replica DFS?

L'elenco seguente include un set di linee guida sulla scalabilità che sono state testate da Microsoft e si applicano a Windows Server 2012 R2, Windows Server 2016 e Windows Server 2019

  - Dimensione di tutti i file replicati in un server: 100 terabyte.  
      
  - Numero di file replicati in un volume: 70 milioni.  
      
  - Dimensione massima dei file: 250 gigabyte.  
      


> [!IMPORTANT]
> Quando vengono creati gruppi di replica con un numero elevato di file o con file di grandi dimensioni, consigliamo di esportare un clone del database e usare tecniche di pre-seeding per ridurre al minimo la durata della replica iniziale. Per altre informazioni, vedi [Sincronizzazione iniziale di Replica DFS in Windows Server 2012 R2: attacco dei cloni](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/DFS-Replication-Initial-Sync-in-Windows-Server-2012-R2-Attack-of/ba-p/424877). 
<br>


L'elenco seguente include un set di linee guida sulla scalabilità che sono state testate da Microsoft su Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008:

  - Dimensione di tutti i file replicati in un server: 10 terabyte.  
      
  - Numero di file replicati in un volume: 11 milioni.  
      
  - Dimensione massima dei file: 64 gigabyte.  
      


> [!NOTE]
> Non esiste più un limite per il numero di gruppi di replica, cartelle replicate, connessioni o membri di gruppo di replica. 
<br>


Per un elenco delle linee guida sulla scalabilità testate da Microsoft per Windows Server 2003 R2, vedi [Linee guida sulla scalabilità di Replica DFS](https://go.microsoft.com/fwlink/?linkid=75043) (https://go.microsoft.com/fwlink/?LinkId=75043).

### <a name="when-should-i-not-use-dfs-replication"></a>Quando non è consigliabile usare Replica DFS?

Non usare Replica DFS in un ambiente in cui più utenti aggiornano o modificano contemporaneamente gli stessi file in server diversi. In situazioni di questo tipo, Replica DFS può spostare copie in conflitto dei file nella cartella nascosta DfsrPrivate\\ConflictAndDeleted.

Quando più utenti devono modificare contemporaneamente gli stessi file in server diversi, usa la funzionalità di estrazione dei file di Windows SharePoint Services per avere la certezza che un file venga modificato da un solo utente alla volta. Windows SharePoint Services 2.0 con Service Pack 2 è disponibile come componente di Windows Server 2003 R2. Windows SharePoint Services può essere scaricato dal sito Web Microsoft, ma non è incluso nelle versioni più recenti di Windows Server.

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>Perché è necessario eseguire un aggiornamento dello schema per Replica DFS?

Replica DFS usa nuovi oggetti nel contesto dei nomi di dominio di Active Directory Domain Services per archiviare le informazioni di configurazione. Questi oggetti vengono creati quando aggiorni lo schema di Active Directory Domain Services. Per altre informazioni, vedi [Rivedere i requisiti per Replica DFS](https://go.microsoft.com/fwlink/?linkid=182264) (https://go.microsoft.com/fwlink/?LinkId=182264).

## <a name="monitoring-and-management-tools"></a>Strumenti di monitoraggio e gestione

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>Posso automatizzare la generazione dei rapporti di stato in modo da ricevere avvisi?

Sì. Sono disponibili tre modi per automatizzare la generazione dei rapporti di stato:

  - Puoi usare il modulo DFSR di Windows PowerShell incluso in Windows Server 2012 R2 o DfsrAdmin.exe insieme ad Attività pianificate per generare regolarmente rapporti di stato. Per altre informazioni, vedi [Automazione dei rapporti di stato di Replica DFS](https://go.microsoft.com/fwlink/?linkid=74010) (https://go.microsoft.com/fwlink/?LinkId=74010).  
      
  - Puoi usare DFS Replication Management Pack for System Center Operations Manager per creare avvisi in base alle condizioni specificate.  
      
  - Puoi usare il provider WMI di Replica DFS per creare script per gli avvisi.  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>Posso usare Microsoft System Center Operations Manager per monitorare Replica DFS?

Sì. Per altre informazioni, vedi [DFS Replication Management Pack for System Center Operations Manager 2007](https://go.microsoft.com/fwlink/?linkid=182265) nell'Area download Microsoft (https://go.microsoft.com/fwlink/?LinkId=182265).

### <a name="does-dfs-replication-support-remote-management"></a>Replica DFS supporta la gestione remota?

Sì. Replica DFS supporta la gestione remota tramite la console Gestione DFS e il comando **Aggiungi gruppo di replica**. Ad esempio, nel server A puoi connetterti a un gruppo di replica definito nella foresta con i server A e B come membri.

La console Gestione DFS è inclusa in Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 e Windows Server 2003 R2. Per gestire Replica DFS da altre versioni di Windows, usa Desktop remoto o [Strumenti di amministrazione remota del server per Windows 7](https://technet.microsoft.com/library/Ee449475).


> [!IMPORTANT]
> Per visualizzare o gestire i gruppi di replica che contengono cartelle replicate di sola lettura o membri che sono cluster di failover, devi usare la versione di Gestione DFS inclusa in Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, <a href="https://go.microsoft.com/fwlink/p/?linkid=238560">Strumenti di amministrazione remota del server per Windows 8</a> o <a href="https://technet.microsoft.com/library/ee449475">Strumenti di amministrazione remota del server per Windows 7</a>. 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>Ultrasound e Sonar funzionano con Replica DFS?

No. Replica DFS dispone di un set specifico di strumenti di monitoraggio e diagnostica. Ultrasound e Sonar sono in grado di monitorare solo il servizio Replica file.

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>In che modo posso ripristinare i file dalle cartelle ConflictAndDeleted o PreExisting?

Per recuperare i file persi, ripristina i file dalla cartella del file system o dalla cartella condivisa usando Cronologia file o il comando **Ripristina versioni precedenti** disponibile in Esplora file oppure ripristinando i file dal backup. Per ripristinare i file direttamente dalla cartella ConflictAndDeleted o PreExisting, usa i cmdlet `Get-DfsrPreservedFiles` e `Restore-DfsrPreservedFiles` di Windows PowerShell (inclusi nel modulo DFSR in Windows Server 2012 R2) o lo script di esempio [RestoreDFSR](https://code.msdn.microsoft.com/restoredfsr) disponibile in MSDN Code Gallery. Questo script è destinato esclusivamente al ripristino di emergenza e viene fornito così com'è, senza garanzie.

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>Esiste un modo per conoscere lo stato della replica?

Sì. Sono disponibili diversi modi per monitorare la replica:

  - Replica DFS dispone di un Management Pack per System Center Operations Manager che fornisce funzionalità di monitoraggio proattivo.  
      
  - In Gestione DFS può essere generato un rapporto di diagnostica per il backlog di replica, l'efficienza della replica e il numero di file e cartelle in un determinato gruppo di replica.  
      
  - Il modulo DFSR di Windows PowerShell in Windows Server 2012 R2 contiene i cmdlet per l'avvio dei test di propagazione e la scrittura di rapporti di stato e propagazione. Per altre informazioni, vedi [Cmdlet di Replica DFS in Windows PowerShell](https://technet.microsoft.com/library/dn296601.aspx).  
      
  - Dfsrdiag.exe è uno strumento da riga di comando che può generare un conteggio di backlog o attivare un test di propagazione. In entrambi è riportato lo stato della replica. La propagazione mostra se i file vengono replicati in tutti i nodi. Il backlog mostra il numero di file che devono essere ancora replicati prima che due computer siano sincronizzati. Il conteggio del backlog è il numero di aggiornamenti non elaborati da un membro del gruppo di replica. Nei computer che eseguono Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2, Dfsrdiag.exe può anche visualizzare gli aggiornamenti che Replica DFS sta attualmente replicando.  
      
  - Gli script possono usare WMI per raccogliere informazioni sul backlog, manualmente o tramite MOM.  
      

## <a name="performance"></a>Prestazioni

### <a name="does-dfs-replication-support-dial-up-connections"></a>Le connessioni remote sono supportate da Replica DFS?

Anche se Replica DFS è in grado di funzionare alle velocità delle connessioni remote, se il numero di modifiche da replicare è elevato può verificarsi un backlog. In caso di piccole modifiche ai file esistenti, Replica DFS con RDC (Remote Differential Compression) è in grado di offrire prestazioni molto più elevate rispetto alla copia diretta del file.

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>Replica DFS esegue il rilevamento della larghezza di banda?

No. Replica DFS non esegue il rilevamento della larghezza di banda. Puoi configurare Replica DFS per l'uso di una quantità limitata di larghezza di banda in base alla connessione (limitazione della larghezza di banda). Replica DFS non riduce tuttavia l'utilizzo della larghezza di banda in caso di saturazione dell'interfaccia di rete e può saturare il collegamento per brevi periodi. La limitazione della larghezza di banda con Replica DFS non è del tutto precisa perché avviene tramite la limitazione delle chiamate RPC. Di conseguenza, vari buffer nei livelli inferiori dello stack di rete (incluso RPC) potrebbero interferire, causando picchi del traffico di rete.

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>Replica DFS limita la larghezza di banda in base a pianificazione, server o connessione?

Se configuri la limitazione della larghezza di banda quando specifichi la pianificazione, tutte le connessioni per il gruppo di replica useranno tale impostazione per la limitazione della larghezza di banda. La limitazione della larghezza di banda può essere impostata anche a livello di connessione tramite Gestione DFS.

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>Replica DFS usa Active Directory Domain Services per calcolare i collegamenti di sito e i costi di connessione?

No. Replica DFS usa la topologia definita dall'amministratore, che è indipendente dalla determinazione dei costi di sito di Active Directory Domain Services.

### <a name="how-can-i-improve-replication-performance"></a>Come posso migliorare le prestazioni della replica?

Per informazioni sui diversi metodi di ottimizzazione delle prestazioni della replica, vedi [Ottimizzazione delle prestazioni della replica in DFSR](https://blogs.technet.com/b/askds/archive/2010/03/31/tuning-replication-performance-in-dfsr-especially-on-win2008-r2.aspx) nel [blog del team di Directory Services](https://blogs.technet.com/b/askds/).

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>In che modo Replica DFS evita di saturare una connessione?

In Replica DFS puoi impostare la larghezza di banda massima da usare per una connessione e il servizio mantiene tale livello di utilizzo della rete. Questa impostazione è diversa da quella del Servizio trasferimento intelligente in background (BITS) e Replica DFS non satura la connessione se l'impostazione è definita in modo appropriato.

Tuttavia, la limitazione della larghezza di banda non è precisa al 100% e Replica DFS può saturare il collegamento per brevi periodi di tempo. Questo avviene perché Replica DFS limita la larghezza di banda tramite le chiamate RPC. Poiché questo processo si basa su vari buffer nei livelli inferiori dello stack di rete, incluso RPC, il traffico di replica tende ad avere bruschi picchi e questo talvolta può saturare i collegamenti di rete.

Replica DFS in Windows Server 2008 include diversi miglioramenti delle prestazioni, come illustrato nell'argomento [File system distribuito](https://technet.microsoft.com/library/Cc753479) in [Modifiche apportate alle funzionalità da Windows Server 2003 con SP1 a Windows Server 2008](https://technet.microsoft.com/library/cc753208).

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>Qual è la differenza tra le prestazioni di Replica DFS e quelle del servizio Replica file?

Replica DFS è molto più veloce rispetto al servizio Replica file, in particolare quando vengono apportate piccole modifiche a file di grandi dimensioni e la tecnologia RDC è abilitata. Con RDC, ad esempio, una piccola modifica a una presentazione di PowerPoint&reg; da 2 MB può determinare il trasferimento di soli 60 kilobyte (KB) attraverso la rete, con un risparmio del 97% in termini di byte trasferiti.

La tecnologia RDC non viene usata su file di dimensioni inferiori a 64 kB e potrebbe non essere vantaggiosa sulle reti LAN ad alta velocità in cui non si verificano problemi di contesa della larghezza di banda della rete. RDC può essere disabilitata in base alla connessione tramite Gestione DFS.

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>Con quale frequenza viene eseguita la replica dei dati tramite Replica DFS?

I dati vengono replicati in base alla pianificazione impostata. Puoi ad esempio impostare la pianificazione in base a intervalli di 15 minuti, per sette giorni alla settimana. Durante questi intervalli la replica è abilitata. La replica viene avviata subito dopo il rilevamento di una modifica di file (in genere nel giro di secondi).

La pianificazione del gruppo di replica può essere impostata su UTC (Universal Time Coordinate) mentre quella della connessione è impostata sull'ora locale del membro di destinazione. Tieni presente questa differenza quando il gruppo di replica si estende su più fusi orari. L'ora locale corrisponde all'ora del membro che ospita la connessione in ingresso. La pianificazione visualizzata per la connessione in ingresso e quella della corrispondente connessione in uscita riflettono le differenze di fuso orario in caso di pianificazione impostata sull'ora locale.

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>Quante risorse di sistema del server verranno utilizzate da Replica DFS?

Le risorse del disco, della memoria e della CPU utilizzate da Replica DFS dipendono da diversi fattori, tra cui il numero e le dimensioni dei file, la frequenza delle modifiche, il numero di membri del gruppo di replica e il numero di cartelle replicate. Inoltre, la stima di alcune risorse può risultare più difficile. La tecnologia Extensible Storage Engine (ESE) usata per il database di Replica DFS, ad esempio, può utilizzare un'alta percentuale di memoria disponibile, che rilascia su richiesta. Nello stesso server di Replica DFS possono essere ospitate anche altre applicazioni, a seconda della configurazione del server. Nel caso di hosting di più applicazioni o ruoli in un singolo server, tuttavia, è importante testare la configurazione prima di implementarla in un ambiente di produzione.

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>Cosa accade se un collegamento WAN non riesce durante la replica?

Se la connessione viene interrotta, Replica DFS ripeterà i tentativi di replica finché la pianificazione rimarrà aperta. Gli errori di connettività verranno inoltre riportati nel registro eventi di Replica DFS, le cui informazioni possono essere raccolte usando MOM (in modo proattivo, tramite avvisi) e il rapporto di stato di Replica DFS (in modo reattivo, da parte di un amministratore).

## <a name="remote-differential-compression-details"></a>Dettagli relativi alla tecnologia RDC

### <a name="are-changes-compressed-before-being-replicated"></a>Le modifiche vengono compresse prima di essere replicate?

Sì. Le parti modificate dei file vengono compresse prima di essere inviate per tutti i tipi di file tranne i seguenti (che sono già compressi): .wma, .wmv, .zip, .jpg, .mpg, .mpeg, .m1v, .mp2, .mp3, .mpa, .cab, .wav, .snd, .au, .asf, .wm, .avi, .z, .gz, .tgz e .frx. Le impostazioni di compressione per questi tipi di file non sono configurabili in Windows Server 2003 R2.

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>Un amministratore può disabilitare RDC o modificare la soglia?

Sì. Puoi disabilitare la tecnologia RDC tramite la pagina delle proprietà di una determinata connessione. La disabilitazione di RDC può ridurre l'utilizzo della CPU e la latenza di replica sui collegamenti LAN (Local Area Network) veloci che non hanno vincoli di larghezza di banda o per i gruppi di replica costituiti principalmente da file di dimensioni inferiori a 64 kB. Se scegli di disabilitare RDC su una connessione, esegui il test dell'efficienza della replica prima e dopo la modifica per verificare di aver migliorato le prestazioni.

Puoi modificare la soglia delle dimensioni per RDC usando il comando **Dfsradmin Connection Set** o il provider WMI di Replica DFS oppure modificando manualmente il file XML di configurazione.

### <a name="does-rdc-work-on-all-file-types"></a>RDC funziona su tutti i tipi di file?

Sì. La tecnologia RDC calcola le differenze a livello di blocco indipendentemente dal tipo di dati del file. RDC funziona tuttavia in modo più efficiente per determinati tipi di file, ad esempio documenti di Word, file PST e immagini VHD.

### <a name="how-does-rdc-work-on-a-compressed-file"></a>Come funziona RDC su un file compresso?

Replica DFS usa la tecnologia RDC, che calcola i blocchi modificati nel file e invia solo tali blocchi attraverso la rete. Replica DFS non deve conoscere il contenuto del file, ma solo i blocchi che sono stati modificati.

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>La tecnologia RDC tra file viene abilitata in caso di aggiornamento a Windows Server Enterprise Edition o Datacenter Edition?

Le edizioni standard di Windows Server non supportano la tecnologia RDC tra file. Questa tecnologia viene tuttavia abilitata automaticamente in caso di aggiornamento a un'edizione che supporta RDC tra file oppure se un membro della connessione di replica esegue un'edizione supportata. Per un elenco delle edizioni che supportano RDC tra file, vedi Quali edizioni del sistema operativo Windows supportano la tecnologia RDC tra file?

### <a name="is-rdc-true-block-level-replication"></a>RDC è una vera e propria replica a livello di blocco?

No. RDC è un protocollo di utilizzo generico per la compressione del trasferimento di file. Replica DFS usa RDC sui blocchi a livello di file e non a livello di blocchi del disco. RDC divide un file in blocchi. Per ogni blocco di un file, viene calcolata una firma, ovvero un numero ridotto di byte che può rappresentare il blocco più grande. Il set di firme viene trasferito dal server al client. Il client confronta le firme del server con le proprie. Il client richiede quindi al server di inviare solo i dati per le firme che non sono già presenti nel client.

### <a name="what-happens-if-i-rename-a-file"></a>Che cosa accade se si rinomina un file?

Replica DFS rinomina il file in tutti gli altri membri del gruppo di replica durante la replica successiva. I file vengono tracciati con un ID univoco e pertanto la ridenominazione di un file e lo spostamento del file all'interno della replica non influisce sulla capacità di Replica DFS di replicare un file.

### <a name="what-is-cross-file-rdc"></a>In cosa consiste la tecnologia RDC tra file?

La tecnologia RDC tra file consente a Replica DFS di usare RDC anche quando non esiste un file con lo stesso nome sul lato client. Questa tecnologia usa un'euristica per determinare i file che sono simili a quello da replicare e usa i blocchi dei file simili che sono identici al file da replicare in modo da ridurre al minimo la quantità di dati trasferiti attraverso la rete WAN. In questo processo RDC tra file può usare blocchi costituiti da un massimo di cinque file simili.

Per usare RDC tra file, un membro della connessione di replica deve eseguire un'edizione di Windows che supporta questa tecnologia. Per un elenco delle edizioni che supportano RDC tra file, vedi Quali edizioni del sistema operativo Windows supportano la tecnologia RDC tra file?

### <a name="what-is-rdc"></a>Che cos'è RDC?

RDC (Remote Differential Compression) è un protocollo client-server che può essere usato per aggiornare in modo efficiente i file attraverso una rete con larghezza di banda limitata. RDC rileva eventuali inserimenti, rimozioni e ridisposizioni dei dati nei file, consentendo a Replica DFS di replicare le modifiche solo in caso di aggiornamento dei file. Per impostazione predefinita, la tecnologia RDC viene usata solo per i file di dimensione pari o superiore a 64 kB. RDC può usare una versione precedente di un file con lo stesso nome nella cartella replicata o nella cartella DfsrPrivate\\ConflictandDeleted (nel percorso locale della cartella replicata).

### <a name="when-is-rdc-used-for-replication"></a>Quando viene usata la tecnologia RDC per la replica?

La tecnologia RDC viene usata quando il file supera una soglia di dimensione minima. Per impostazione predefinita, questa soglia è pari a 64 kB. Dopo che un file che supera tale soglia è stato replicato, le versioni aggiornate del file usano sempre RDC, a meno che non venga modificata una parte significativa del file o la tecnologia RDC venga disabilitata.

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>Quali edizioni del sistema operativo Windows supportano la tecnologia RDC tra file?

Per usare RDC tra file, un membro della connessione di replica deve eseguire un'edizione del sistema operativo Windows che supporta questa tecnologia. La tabella seguente mostra le edizioni del sistema operativo Windows che supportano la tecnologia RDC tra file.

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
<td><p>Sì</em></p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012</p></td>
<td><p>Sì</p></td>
<td><p>Non disponibile</p></td>
<td><p>Sì</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2008 R2</p></td>
<td><p>No</p></td>
<td><p>Sì</p></td>
<td><p>Sì</p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2008</p></td>
<td><p>No</p></td>
<td><p>Sì</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2003 R2</p></td>
<td><p>No</p></td>
<td><p>Sì</p></td>
<td><p>No</p></td>
</tr>
</tbody>
</table>

\* Facoltativamente, puoi disabilitare RDC tra file in Windows Server 2012 R2.

## <a name="replication-details"></a>Dettagli relativi alla replica

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>Posso modificare il percorso di una cartella replicata dopo che è stata creata?

No. Per modificare il percorso di una cartella replicata, devi eliminarla in Gestione DFS e aggiungerla nuovamente come nuova cartella replicata. Replica DFS usa quindi RDC per eseguire una sincronizzazione che determina se i dati dei membri di origine e di destinazione sono gli stessi. Non esegue di nuovo la replica di tutti i dati nella cartella.

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>Posso configurare gli attributi di file da replicare?

No. Non è possibile configurare gli attributi di file replicati da Replica DFS.

Per un elenco di valori di attributo con le relative descrizioni, vedi [Attributi di file](https://go.microsoft.com/fwlink/?linkid=182268) su MSDN (https://go.microsoft.com/fwlink/?LinkId=182268).

I valori degli attributi seguenti vengono impostati tramite la funzione `SetFileAttributes dwFileAttributes` e replicati da Replica DFS. Le modifiche a questi valori attivano la replica degli attributi. Il contenuto del file non viene replicato, a meno che non sia stato modificato. Per altre informazioni, vedi [Funzione SetFileAttributes](https://go.microsoft.com/fwlink/?linkid=182269) in MSDN Library (https://go.microsoft.com/fwlink/?LinkId=182269).

  - FILE\_ATTRIBUTE\_HIDDEN  
      
  - FILE\_ATTRIBUTE\_READONLY  
      
  - FILE\_ATTRIBUTE\_SYSTEM  
      
  - FILE\_ATTRIBUTE\_NOT\_CONTENT\_INDEXED  
      
  - FILE\_ATTRIBUTE\_OFFLINE  
      

I valori degli attributi seguenti vengono replicati da Replica DFS, ma non attivano la replica.

  - FILE\_ATTRIBUTE\_ARCHIVE  
      
  - FILE\_ATTRIBUTE\_NORMAL  
      

Anche i valori degli attributi di file seguenti attivano la replica, sebbene non possano essere impostati tramite la funzione `SetFileAttributes`. Per visualizzare i valori degli attributi è disponibile la funzione `GetFileAttributes`.

  - FILE\_ATTRIBUTE\_REPARSE\_POINT  
      

> [!NOTE]
> Replica DFS non esegue la replica dei valori dell'attributo reparse point, a meno che il tag reparse non sia IO_REPARSE_TAG_SYMLINK. I file con i tag reparse IO_REPARSE_TAG_DEDUP, IO_REPARSE_TAG_SIS o IO_REPARSE_TAG_HSM vengono replicati come i file normali. Il tag reparse e i buffer dei dati reparse non vengono tuttavia replicati in altri server perché il reparse point funziona solo sul sistema locale. 
<br>

  - FILE\_ATTRIBUTE\_COMPRESSED  
      
  - FILE\_ATTRIBUTE\_ENCRYPTED  
      

> [!NOTE]
> Replica DFS non esegue la replica di file crittografati con Encrypting File System (EFS). Replica DFS esegue la replica di file crittografati con software non Microsoft, ma solo se non imposta il valore dell'attributo FILE_ATTRIBUTE_ENCRYPTED sul file. 
<br>

  - FILE\_ATTRIBUTE\_SPARSE\_FILE  
      
  - FILE\_ATTRIBUTE\_DIRECTORY  
      

Replica DFS non esegue la replica del valore di FILE\_ATTRIBUTE\_TEMPORARY.

### <a name="can-i-control-which-member-is-replicated"></a>Posso controllare quale membro viene replicato?

Sì. Quando crei un gruppo di replica puoi scegliere una topologia. In alternativa, puoi selezionare **Nessuna topologia** e configurare manualmente le connessioni dopo che il gruppo di replica è stato creato.

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>Posso eseguire il seeding di un membro del gruppo di replica con i dati presenti prima della replica iniziale?

Sì. Replica DFS supporta la copia dei file in un membro del gruppo di replica prima della replica iniziale. Questo processo di pre-installazione può ridurre notevolmente la quantità di dati replicati durante la replica iniziale.

La replica iniziale non deve replicare il contenuto quando i file differiscono solo per attributi reali o timestamp. Un attributo reale è un attributo che può essere impostato dalla funzione Win32 `SetFileAttributes`. Per altre informazioni, vedi [Funzione SetFileAttributes](https://go.microsoft.com/fwlink/?linkid=182269) in MSDN Library (https://go.microsoft.com/fwlink/?LinkId=182269). Se due file differiscono per altri attributi, ad esempio la compressione, il contenuto del file viene replicato.

Per pre-installare un membro del gruppo di replica, copia i file nella cartella appropriata nei server di destinazione, crea il gruppo di replica e quindi scegli un membro primario. Scegli il membro contenente i file più aggiornati che vuoi replicare, poiché il contenuto del membro primario viene considerato "autorevole". Ciò significa che durante la replica iniziale, i file del membro primario sovrascriveranno sempre le altre versioni dei file sugli altri membri del gruppo di replica.

Per informazioni sulla pre-installazione e sulla clonazione del database DFSR, vedi [Sincronizzazione iniziale di Replica DFS in Windows Server 2012 R2: attacco dei cloni](https://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx).

Per altre informazioni sulla replica iniziale, vedi [Creare un gruppo di replica](https://technet.microsoft.com/library/cc725893).

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>Replica DFS offre una soluzione ai problemi comuni del servizio Replica file?

Sì. Replica DFS risolve tre problemi comuni del servizio Replica file:

  - Troncamenti del journal: Replica DFS esegue il ripristino in tempo reale dei troncamenti del journal. Ogni file o cartella esistente viene contrassegnata come journalWrap e verificata in base al file system prima che la replica sia nuovamente abilitata. Durante il ripristino, questo volume non è disponibile per la replica in entrambe le direzioni.  
      
  - Replica eccessiva: per evitare un numero eccessivo di processi di replica, Replica DFS usa un sistema di crediti.  
      
  - Cartelle modificate: per evitare i nomi di cartella modificati, Replica DFS archivia i dati in conflitto in una cartella nascosta DfsrPrivate\\ConflictandDeleted (nel percorso locale della cartella replicata). Se ad esempio crei simultaneamente più cartelle con nomi identici su server diversi replicati con il servizio Replica file, le cartelle vengono rinominate. Diversamente da questo servizio, Replica DFS sposta le cartelle precedenti nella cartella locale dei file eliminati o con conflitti.  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>Replica DFS esegue la replica dei file in ordine cronologico?

No. I file possono essere replicati senza alcun ordine.

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>Replica DFS esegue la replica dei file che vengono usati da un'altra applicazione?

Se un'applicazione apre un file e crea un blocco su di esso, impedendone l'uso da parte di altre applicazioni mentre è aperto, Replica DFS non eseguirà la replica del file finché non viene chiuso. Se l'applicazione apre il file con accesso in lettura e condivisione, il file può comunque essere replicato.

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>Replica DFS esegue la replica delle autorizzazioni per i file NTFS, dei flussi di dati alternativi, dei collegamenti reali e dei reparse point?

  - Replica DFS esegue la replica delle autorizzazioni per i file NTFS e dei flussi di dati alternativi.  
      
  - Microsoft non supporta la creazione di collegamenti reali NTFS verso o da file presenti in una cartella replicata. Questa operazione può causare problemi di replica con i file interessati. I file dei collegamenti reali vengono ignorati da Replica DFS e non vengono replicati. Anche i punti di giunzione non vengono replicati e Replica DFS registra l'evento 4406 per ogni punto di giunzione rilevato.  
      
  - Gli unici reparse point replicati da Replica DFS sono quelli che usano il tag IO\_REPARSE\_TAG\_SYMLINK, anche se non è garantito che venga replicata anche la destinazione di un collegamento simbolico. Per altre informazioni, vedi il [blog del team di Directory Services](https://blogs.technet.com/b/askds/archive/2011/09/30/friday-mail-sack-super-slo-mo-edition.aspx).  
      
  - I file con i tag reparse IO\_REPARSE\_TAG\_DEDUP, IO\_REPARSE\_TAG\_SIS o IO\_REPARSE\_TAG\_HSM vengono replicati come file normali. Il tag reparse e i buffer dei dati reparse non vengono replicati in altri server perché il reparse point funziona solo sul sistema locale. Replica DFS può quindi replicare le cartelle nei volumi che usano Deduplicazione dati in Windows Server 2012 o Single Instance Storage (SIS), anche se le informazioni di deduplicazione dei dati vengono gestite separatamente da ogni server su cui è abilitato il servizio ruolo.  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>Replica DFS esegue la replica delle modifiche di timestamp se non sono state apportate altre modifiche al file?

No. Replica DFS non esegue la replica dei file per i quali è stato modificato solo il timestamp. Inoltre, il timestamp modificato non viene replicato in altri membri del gruppo di replica, a meno che non vengano apportate altre modifiche al file.

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>Replica DFS esegue la replica delle autorizzazioni aggiornate per un file o una cartella?

Sì. Replica DFS esegue la replica delle modifiche apportate alle autorizzazioni per file e cartelle. Solo la parte del file associata all'elenco di controllo di accesso (ACL) viene replicata, sebbene Replica DFS debba comunque leggere l'intero file nell'area di gestione temporanea.


> [!NOTE]
> La modifica degli ACL per un numero elevato di file può influire sulle prestazioni della replica. Tuttavia, quando viene usata la tecnologia RDC, la quantità di dati trasferiti è proporzionale alla dimensione degli ACL e non a quella dell'intero file. La quantità di traffico su disco è comunque proporzionale alla dimensione dei file perché questi devono essere letti da e verso la cartella di gestione temporanea. 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>Replica DFS supporta l'unione di file di testo in caso di conflitto?

Replica DFS non unisce i file quando si verifica un conflitto, ma tenta di mantenere la versione precedente del file nella cartella nascosta DfsrPrivate\\ConflictAndDeleted nel computer in cui il conflitto è stato rilevato.

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>Replica DFS usa la crittografia durante la trasmissione dei dati?

Sì. Replica DFS usa connessioni RPC (Remote Procedure Call) con crittografia.

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>È possibile disabilitare l'uso di connessioni RPC crittografate?

No. Il servizio Replica DFS usa RPC su TCP per replicare i dati. Per proteggere i trasferimenti di dati attraverso Internet, il servizio Replica DFS è stato progettato in modo da usare sempre la costante a livello di autenticazione, `RPC_C_AUTHN_LEVEL_PKT_PRIVACY`. Questo garantisce che le comunicazioni RPC su Internet siano sempre crittografate. Non è pertanto possibile disabilitare l'uso di connessioni RPC crittografate per il servizio Replica DFS.

Per altre informazioni, visita il sito Web Microsoft seguente:

  - [Documentazione tecnica su RPC](https://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [Informazioni su RDC (Remote Differential Compression)](https://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [Costanti a livello di autenticazione](https://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>Come vengono gestite le repliche simultanee?

Viene eseguita una gestione degli aggiornamenti per ogni cartella replicata. Le attività di gestione degli aggiornamenti vengono eseguite in modo indipendente l'una dall'altra.

Per impostazione predefinita, tra tutte le connessioni e tutti i gruppi di replica vengono condivisi al massimo 16 (quattro in Windows Server 2003 R2) download simultanei. Poiché le connessioni e gli aggiornamenti dei gruppi di replica non sono serializzati, non esiste un ordine specifico in cui vengono ricevuti gli aggiornamenti. Se vengono aperte due pianificazioni, gli aggiornamenti vengono in genere ricevuti e installati contemporaneamente da entrambe le connessioni.

### <a name="how-do-i-force-replication-or-polling"></a>Come posso forzare la replica o il polling?

Puoi forzare la replica immediatamente usando Gestione DFS, come descritto in [Modificare le pianificazioni di replica](https://technet.microsoft.com/library/Cc732278). Per forzare la replica puoi anche usare il cmdlet `Sync-DfsReplicationGroup`, incluso nel modulo DFSR di PowerShell introdotto con Windows Server 2012 R2 oppure il comando **Dfsrdiag SyncNow**. Per forzare il polling puoi usare il cmdlet `Update-DfsrConfigurationFromAD` oppure il comando **Dfsrdiag PollAD**.

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>È possibile configurare un tempo di attesa tra le repliche per i file che cambiano di frequente?

No. Se la pianificazione è aperta, il servizio Replica DFS esegue la replica delle modifiche non appena le rileva. Non è possibile configurare un tempo di attesa per i file.

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>È possibile configurare la replica unidirezionale con Replica DFS?

Sì. Se usi Windows Server 2012 o Windows Server 2008 R2, puoi creare una cartella replicata di sola lettura che replica il contenuto attraverso una connessione unidirezionale. Per altre informazioni, vedi [Impostare una cartella replicata di sola lettura in un membro specifico](https://go.microsoft.com/fwlink/?linkid=156740) (https://go.microsoft.com/fwlink/?LinkId=156740).

In Windows Server 2008 o Windows Server 2003 R2 non è supportata la creazione di una connessione di replica unidirezionale con Replica DFS. Una connessione di questo tipo può causare numerosi problemi, quali errori di topologia del controllo di integrità, problemi di gestione temporanea e problemi con il database di Replica DFS.

Se usi Windows Server 2008 o Windows Server 2003 R2, puoi simulare una connessione unidirezionale eseguendo le azioni seguenti:

  - Fornisci istruzioni agli amministratori in modo che vengano apportate modifiche solo ai server che vuoi designare come primari e quindi lascia che le modifiche vengano replicate nei server di destinazione.  
      
  - Configura le autorizzazioni di condivisione nei server di destinazione in modo che gli utenti finali non dispongano delle autorizzazioni di scrittura. Se non sono consentite modifiche nei server delle sedi secondarie, non vi saranno dati da replicare. In questo modo, simuli una connessione unidirezionale limitando l'utilizzo della rete WAN.  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>Esiste un modo per forzare una replica completa di tutti i file, inclusi quelli non modificati?

No. Se Replica DFS rileva che i file sono identici, non ne esegue la replica. Se i file modificati non sono stati replicati, Replica DFS ne esegue automaticamente la replica quando è configurato per eseguire questa operazione. Per sovrascrivere la pianificazione configurata, usa il metodo WMI **ForceReplicate()** . Questa operazione è comunque solo un override della pianificazione e non ha l'effetto di forzare la replica di file non modificati o identici.

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>Cosa accade se sul membro primario si verifica una perdita di database durante la replica iniziale?

Durante la replica iniziale i file del membro primario hanno sempre la precedenza nella risoluzione dei conflitti che si verificano se i membri di destinazione hanno versioni diverse dei file rispetto al membro primario. La designazione di membro primario è archiviata in Active Directory Domain Services e viene cancellata dopo che il membro primario è pronto per la replica, ma prima della replica di tutti i membri del gruppo di replica.

Se la replica iniziale ha esito negativo o il servizio Replica DFS viene riavviato durante la replica, il membro primario rileva la designazione di membro primario nel database di Replica DFS locale e prova nuovamente a eseguire la replica iniziale. Se il database di Replica DFS del membro primario viene perso dopo la cancellazione della designazione primaria in Active Directory Domain Services, ma prima che tutti i membri del gruppo di replica completino la replica iniziale, tali membri non riescono a replicare la cartella perché nessun server è designato come membro primario. Se si verifica una situazione di questo tipo, usa il comando **Dfsradmin membership /set /isprimary:true** sul server membro primario per ripristinare manualmente la designazione di membro primario.

Per altre informazioni sulla replica iniziale, vedi [Creare un gruppo di replica](https://technet.microsoft.com/library/cc725893).


> [!WARNING]
> La designazione di membro primario viene usata solo durante il processo di replica iniziale. Se usi il comando <STRONG>Dfsradmin</STRONG> per specificare un membro primario per una cartella replicata al termine della replica, il servizio Replica DFS non designa il server come membro primario in Active Directory Domain Services. Se tuttavia il database di Replica DFS sul server subisce successivamente un danno irreversibile o una perdita di dati, il server tenta di eseguire una replica iniziale come membro primario anziché recuperare i dati da un altro membro del gruppo di replica. In sostanza, il server diventa un server primario non autorizzato, che può causare conflitti. Per questo motivo, specifica manualmente il membro primario solo se hai la certezza che la replica iniziale ha avuto irrimediabilmente esito negativo. 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>Cosa accade se la pianificazione della replica viene chiusa mentre è in corso la replica di un file?

Se per la connessione è abilitata la tecnologia RDC, la replica in ingresso di un file di dimensione superiore a 64 kB che è iniziata immediatamente prima della chiusura della pianificazione (o del passaggio allo stato con **larghezza di banda assente**) riprende quando la pianificazione viene aperta (o passa a uno stato diverso da **larghezza di banda assente**). La replica riprende dallo stato in cui si trovava quando è stata arrestata.

Se la tecnologia RDC è disabilitata, Replica DFS riavvia completamente il trasferimento dei file. Questo può provocare un ritardo quando il file è disponibile nel membro di destinazione.

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>Cosa accade quando due utenti aggiornano contemporaneamente lo stesso file su server diversi?

Quando Replica DFS rileva un conflitto, usa la versione del file che è stato salvato per ultimo. L'altro file viene spostato nella cartella DfsrPrivate\\ConflictandDeleted, nel percorso locale della cartella replicata sul computer che ha risolto il conflitto. Il file rimane disponibile finché non viene eseguita la pulizia della cartella dei file eliminati o con conflitti, che si verifica quando tale cartella supera la dimensione configurata o Replica DFS rileva un errore di spazio su disco insufficiente. La cartella dei file eliminati o con conflitti non viene replicata e questo metodo di risoluzione dei conflitti evita il problema delle directory modificate che poteva verificarsi nel servizio Replica file.

Quando si verifica un conflitto, Replica DFS registra un evento informativo nel relativo registro eventi. Questo evento non richiede alcun intervento da parte dell'utente per i motivi seguenti:

  - Non è visibile agli utenti, ma solo agli amministratori del server.  
      
  - Replica DFS considera la cartella dei file eliminati o con conflitti come una cache. Quando viene raggiunta una soglia di quota, alcuni dei file vengono eliminati. Non vi è alcuna garanzia che i file in conflitto vengano salvati.  
      
  - Il file in conflitto potrebbe trovarsi in un server diverso dall'origine del conflitto.  
      

## <a name="staging"></a>Gestione temporanea

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>Replica DFS prosegue la gestione temporanea dei file quando la replica viene disabilitata in base a una pianificazione o a una quota di limitazione della larghezza di banda o quando una connessione viene disabilitata manualmente?

No. Replica DFS non prosegue la gestione temporanea dei file al di fuori degli orari di replica pianificati, se è stata superata la quota di limitazione della larghezza di banda o quando le connessioni vengono disabilitate.

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>Replica DFS impedisce ad altre applicazioni di accedere a un file durante la gestione temporanea?

No. Replica DFS apre i file in modo da non impedire agli utenti o alle applicazioni di aprirli nella cartella di replica. Questo metodo è definito "blocco opportunistico".

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>È possibile modificare il percorso della cartella di gestione temporanea con la console Gestione DFS?

Sì. Il percorso della cartella di gestione temporanea è configurato nella scheda **Avanzate** della finestra di dialogo **Proprietà** per ogni membro di un gruppo di replica.

### <a name="when-are-files-staged"></a>Quando viene eseguita la gestione temporanea dei file?

La gestione temporanea dei file viene eseguita sul membro di origine quando tale membro richiede il file, a meno che la relativa dimensione non sia pari o inferiore a 64 kB, come illustrato nella tabella seguente. Se la tecnologia RDC è disabilitata per la connessione, viene eseguita la gestione temporanea del file, a meno che la relativa dimensione non sia pari o inferiore a 256 kB. La gestione temporanea dei file viene eseguita anche sul membro di destinazione durante il trasferimento se la dimensione dei file è inferiore a 64 kB, sebbene sia possibile configurare questa impostazione su un valore compreso tra 16 kB e 1 MB. Se la pianificazione viene chiusa, la gestione temporanea dei file non viene eseguita.

### <a name="the-minimum-file-sizes-for-staging-files"></a>Dimensioni minime dei file per la gestione temporanea

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>RDC abilitata</th>
<th>RDC disabilitata</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>Membro di origine</p></td>
<td><p>64 kB</p></td>
<td><p>256 kB</p></td>
</tr>
<tr class="odd">
<td><p>Membro di destinazione</p></td>
<td><p>64 kB per impostazione predefinita</p></td>
<td><p>64 kB per impostazione predefinita</p></td>
</tr>
</tbody>
</table>

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>Cosa accade se un file viene modificato dopo che ne è stata eseguita la gestione temporanea ma prima che venga completamente trasmesso al sito remoto?

Se una parte del file è già in fase di trasmissione, Replica DFS prosegue la trasmissione. Se il file viene modificato prima che Replica DFS inizi a trasmetterlo, viene inviata la versione più recente del file.

## <a name="change-history"></a>Cronologia delle modifiche


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Data</th>
<th>Description</th>
<th>Motivo</th>
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
<td><p>È stata aggiornata la sezione Quali sono i limiti supportati per Replica DFS? con i risultati dei test eseguiti su Windows Server 2012 R2.</p></td>
<td><p>Aggiornamenti per la versione più recente di Windows Server</p></td>
</tr>
<tr class="odd">
<td><p>30 gennaio 2013</p></td>
<td><p>È stata aggiunta la sezione Replica DFS prosegue la gestione temporanea dei file quando la replica viene disabilitata in base a una pianificazione o a una quota di limitazione della larghezza di banda o quando una connessione viene disabilitata manualmente?</p></td>
<td><p>Domande dei clienti</p></td>
</tr>
<tr class="even">
<td><p>31 ottobre 2012</p></td>
<td><p>È stata modificata la sezione Quali sono i limiti supportati per Replica DFS? per aumentare il numero testato di file replicati in un volume.</p></td>
<td><p>Feedback dei clienti</p></td>
</tr>
<tr class="odd">
<td><p>15 agosto 2012</p></td>
<td><p>È stata modificata la sezione Replica DFS esegue la replica delle autorizzazioni per i file NTFS, dei flussi di dati alternativi, dei collegamenti reali e dei reparse point? per chiarire ulteriormente come Replica DFS gestisce i collegamenti reali e i reparse point.</p></td>
<td><p>Feedback dei servizi di supporto tecnico</p></td>
</tr>
<tr class="even">
<td><p>13 giugno 2012</p></td>
<td><p>È stata modificata la sezione Replica DFS funziona sui volumi ReFS o FAT? per aggiungere le informazioni relative a ReFS.</p></td>
<td><p>Feedback dei clienti</p></td>
</tr>
<tr class="odd">
<td><p>25 aprile 2012</p></td>
<td><p>È stata modificata la sezione Replica DFS esegue la replica delle autorizzazioni per i file NTFS, dei flussi di dati alternativi, dei collegamenti reali e dei reparse point? per chiarire come Replica DFS gestisce i collegamenti reali.</p></td>
<td><p>Ridurre il rischio di possibile confusione</p></td>
</tr>
<tr class="even">
<td><p>30 marzo 2011</p></td>
<td><p>È stata modificata la sezione Replica DFS può replicare i file di database di Microsoft Office Access o i file di Outlook con estensione pst? per correggere il potenziale impatto risultante dall'uso di Replica DFS con i file di Access e i file con estensione pst.</p>
<p>È stata aggiunta la sezione Come posso migliorare le prestazioni della replica?</p></td>
<td><p>Domande dei clienti sulla sezione precedente, in cui era indicato erroneamente che la replica dei file di Access o con estensione pst potrebbe danneggiare il database di Replica DFS.</p></td>
</tr>
<tr class="odd">
<td><p>26 gennaio 2011</p></td>
<td><p>È stata aggiunta la sezione In che modo posso ripristinare i file dalle cartelle ConflictAndDeleted o PreExisting?</p></td>
<td><p>Feedback dei clienti</p></td>
</tr>
<tr class="even">
<td><p>20 ottobre 2010</p></td>
<td><p>È stata aggiunta la sezione Come posso aggiornare o sostituire un membro di Replica DFS?</p></td>
<td><p>Feedback dei clienti</p></td>
</tr>
</tbody>
</table>

