---
ms.assetid: 0f2a7f7b-aca8-4e5d-ad67-4258e88bc52f
title: Novità nell'archiviazione in Windows Server
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dongill
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 05/29/2019
ms.openlocfilehash: 5469d663f64fdb453e03863f409b675473d3f6aa
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308566"
---
# <a name="whats-new-in-storage-in-windows-server"></a>Nuove funzionalità di archiviazione in Windows Server

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale)

In questo argomento illustra le funzionalità nuove e modificate nell'archiviazione in Windows Server 2019, Windows Server 2016 e versioni canale semestrale di Windows Server.

## <a name="whats-new-in-storage-in-windows-server-2019-and-windows-server-version-1903"></a>Novità nell'archiviazione in Windows Server 2019 e Windows Server, versione 1903

Questa versione di Windows Server consente di aggiungere le modifiche e le tecnologie seguenti.

### <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>Il servizio di migrazione di archiviazione ora esegue la migrazione di account locali, cluster e server Linux

Il servizio di migrazione di archiviazione rende più semplice la migrazione di server a una versione più recente di Windows Server. Fornisce uno strumento grafico che archivia i dati nei server, quindi trasferisce i dati e configurazione al server più recenti, tutto senza dover apportare alcuna modifica di utenti o App.

Quando si usa questa versione di Windows Server per orchestrare le migrazioni, sono state aggiunte le possibilità seguenti:

- Eseguire la migrazione di utenti e gruppi locali nel nuovo server
- La migrazione dell'archiviazione dal cluster di failover
- Eseguire la migrazione di archiviazione da un server Linux che usa Samba
- Migrazione di condivisioni di sincronizzazione più facilmente in Azure con sincronizzazione File di Azure
- Eseguire la migrazione a nuove reti, ad esempio Azure

Per altre informazioni sul servizio di migrazione di archiviazione, vedi [Panoramica del servizio di migrazione archiviazione](storage-migration-service/overview.md).

### <a name="system-insights-disk-anomaly-detection"></a>Rilevamento delle anomalie disco del sistema Insights

[Sistema Insights](../manage/system-insights/overview.md) è una funzione analitica predittiva che analizza i dati di sistema di Windows Server in locale e offre informazioni approfondite sul funzionamento del server. È dotato di numerose funzionalità incorporate, ma è stata aggiunta la possibilità per installare funzionalità aggiuntive tramite Windows Admin Center, inizia con il rilevamento delle anomalie del disco.

Rilevamento delle anomalie di disco è una nuova funzionalità che mette in evidenza quando i dischi si comportino *in modo diverso* superiore al consueto. Anche se diverse non è necessariamente negativa, visualizzare questi istanti anomale può essere utile durante la risoluzione dei problemi dei sistemi.

Questa funzionalità è disponibile anche per i server che eseguono Windows Server 2019.

### <a name="windows-admin-center-enhancements"></a>Miglioramenti di Windows Admin Center

Una nuova versione di Windows Admin Center non è più valido, l'aggiunta di nuove funzionalità a Windows Server. Per informazioni sulle funzionalità più recenti, vedi [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md).

## <a name="whats-new-in-storage-in-windows-server-2019-and-windows-server-version-1809"></a>Novità nell'archiviazione in Windows Server 2019 e Windows Server, versione 1809

Questa versione di Windows Server consente di aggiungere le modifiche e le tecnologie seguenti.

### <a name="manage-storage-with-windows-admin-center"></a>Gestire l'archiviazione con Windows Admin Center

[Windows Admin Center](../manage/windows-admin-center/overview.md) è una nuova app distribuite in locale, basato su browser per la gestione di server, i cluster di infrastruttura iperconvergente con spazi di archiviazione diretta, PC e Windows 10. Questa viene fornito senza costi aggiuntivi oltre a Windows ed è pronta per l'uso in produzione.

Per essere onesti, Windows Admin Center è un download separato che viene eseguito in Windows Server 2019 e altre versioni di Windows, ma è nuovo e non vogliamo perdere lo...

### <a name="storage-migration-service"></a>Servizio di migrazione della risorsa di archiviazione

Il servizio di migrazione della risorsa di archiviazione è una nuova tecnologia che semplifica la migrazione dei server a una versione più recente di Windows Server. Fornisce uno strumento grafico che esegue l'inventario dei dati nei server, trasferisce i dati e la configurazione nei server più recenti e, facoltativamente, sposta le identità dei server precedenti ai nuovi server in modo che le app e gli utenti non debbano apportare alcuna modifica. Per altre info, vedi [Servizio di migrazione della risorsa di archiviazione](storage-migration-service/overview.md).

### <a id="storage-spaces-direct"></a>Spazi di archiviazione diretta (solo Windows Server 2019)

Esistono una serie di miglioramenti a spazi di archiviazione diretta in Windows Server 2019 (spazi di archiviazione diretta non è incluso in Windows Server, come canale semestrale):

- **La deduplicazione e compressione per i volumi ReFS**

    Store fino a dieci volte più dati nello stesso volume con la deduplicazione e compressione per il file System (Refs). (Dispone [un solo clic](https://www.youtube.com/watch?v=PRibTacyKko&feature=youtu.be) per attivare con Windows Admin Center.) L'archivio di blocchi di dimensioni variabili con la compressione facoltativa ottimizza i tassi di risparmio, mentre l'architettura di post-elaborazione multithreading mantiene minimo impatto sulle prestazioni. Supporta volumi fino a 64 TB e verrà deduplicare i 4 TB prima di ogni file.

- **Supporto nativo per la memoria persistente**

    Usufruisci di prestazioni senza precedenti con il supporto di Spazi di archiviazione diretta in modalità nativa per moduli di memoria persistente, inclusi Intel® Optane™ DC PM e NVDIMM-n. Usa la memoria persistente come cache per accelerare il working set attivo o come capacità per garantire bassa latenza coerente nell'ordine di microsecondi. Gestisci la memoria persistente esattamente come faresti per qualsiasi altra unità in PowerShell o Windows Admin Center.

- **Resilienza annidata per l'infrastruttura iperconvergente due nodi nella rete perimetrale**

    Supera due errori hardware alla volta con un'opzione di resilienza software completamente nuova ispirata a RAID 5+1. Con la resilienza nidificata, un cluster di Spazi di archiviazione diretta a due nodi può fornire un archivio sempre accessibile per le app e le macchine virtuali anche se un nodo server diventa inattivo e un'unità diventa non disponibile nell'altro nodo server.

- **Cluster di due server con una porta USB, unità memoria flash come server di controllo**

    Usare un'unità flash USB a costo contenuto inserita del router per agire come server di controllo nei due server cluster. Se un server si arresta e quindi eseguire il backup, il cluster di unità USB sappia quale server presenta i dati più aggiornati. Per altre informazioni, vedere la [spazio di archiviazione nel blog di Microsoft](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/).

- **Windows Admin Center**

    Gestisci e monitora Spazi di archiviazione diretta con il nuovo [dashboard specificatamente progettato](../manage/windows-admin-center/use/manage-hyper-converged.md) e l'esperienza in Windows Admin Center. Crea, apri, espandi o elimina volumi con pochi clic. Monitora prestazioni, come la latenza I/O e IOPS dall'intero cluster fino al singolo SSD o HDD. Disponibile senza alcun costo aggiuntivo per Windows Server 2016 e Windows Server 2019.

- **Cronologia delle prestazioni**

    Ottieni facilmente visibilità sull'utilizzo delle risorse e sulle prestazioni con [cronologia predefinita](storage-spaces/performance-history.md). I valori di oltre 50 contatori essenziali, ad esempio relativi a elaborazione, memoria, rete e archiviazione, vengono raccolti e archiviati automaticamente nel cluster fino a un anno. La cosa più interessante è che non c'è niente da installare, configurare o avviare: l'attivazione avviene automaticamente. Visualizza in Windows Admin Center o esegui query ed elabora i risultati in PowerShell.

- **Scalabilità fino a 4 per ogni cluster PB**

    Ottieni un ridimensionamento di più petabyte per supporti di memorizzazione, backup e archiviazione. In Windows Server 2019, Spazi di archiviazione diretta supporta fino a 4 petabyte (PB) = 4.000 TB di capacità non elaborata per pool di archiviazione. Anche le linee guida correlate alla capacità sono maggiori: ad esempio, puoi creare il doppio dei volumi (64 anziché 32), ognuno due volte più grande di prima (64 TB anziché 32 TB). Unire più cluster in un [configurazione del cluster](storage-spaces/cluster-sets.md) anche maggiore scalabilità in uno archiviazione spazio dei nomi. Per altre informazioni, vedere la [spazio di archiviazione nel blog di Microsoft](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/).

- **Parità con accelerazione mirror è 2 volte più veloce**

    Mediante la parità accelerata con mirroring puoi creare volumi con Spazi di archiviazione diretta in grado di offrire in parte mirroring e in parte parità, ad esempio puoi unire RAID-1 e RAID-5/6 per ottenere il meglio di entrambi. (Dispone [più semplice di quanto pensi](https://www.youtube.com/watch?v=R72QHudqWpE) in Windows Admin Center.) In Windows Server 2019, le prestazioni di parità con accelerazione mirror sono più che raddoppiata rispetto al Windows Server 2016 grazie alle ottimizzazioni.

- **Unità di rilevamento degli outlier latenza**

    Identifica con facilità le unità con latenza anomala con monitoraggio proattivo e rilevamento predefinito dell'outlier sulla base dell'approccio consolidato e di successo di Microsoft Azure. Che si tratti della latenza media o qualcosa di più sottile, come la latenza al 99° percentile, le unità lente vengono etichettate automaticamente in PowerShell e Windows Admin Center con lo stato "Latenza anomala".

- **Consente di delimitare manualmente l'allocazione dei volumi per aumentare la tolleranza di errore**

    In questo modo gli amministratori delimitare manualmente l'allocazione dei volumi in spazi di archiviazione diretta. Tale operazione in modo che può aumentare notevolmente la tolleranza di errore in determinate condizioni, ma vengono applicate alcune considerazioni sulla gestione di aggiunta e la complessità. Per altre informazioni, vedi [delimitare l'allocazione dei volumi](storage-spaces/delimit-volume-allocation.md).

### <a name="storage-replica2019"></a>Replica di archiviazione

Esistono una serie di miglioramenti al [Replica di archiviazione](storage-replica/storage-replica-overview.md) in questa versione:

#### <a name="storage-replica-in-windows-server-standard-edition"></a>Replica di archiviazione in Windows Server Standard Edition

È ora possibile usare Replica archiviazione con Windows Server Standard Edition oltre ai Datacenter Edition. Replica di archiviazione in esecuzione in Windows Server Standard Edition, presenta le limitazioni seguenti:

- Replica di archiviazione viene replicato un singolo volume invece di un numero illimitato di volumi.
- I volumi possono avere dimensioni fino a 2 TB invece di dimensioni illimitate.

#### <a name="storage-replica-log-performance-improvements"></a>Miglioramenti delle prestazioni del log per Replica di archiviazione

Sono anche apportati miglioramenti al modo in cui il log di Replica archiviazione tiene traccia della replica, migliorare la velocità effettiva della replica e la latenza, in particolare su archiviazione all-flash, così come i cluster di spazi di archiviazione diretta che replicano tra loro.

Per ottenere le migliori prestazioni, tutti i membri del gruppo di replica devono eseguire Windows Server 2019.

#### <a name="test-failover"></a>Failover di test

È possibile ora temporaneamente montare uno snapshot di archiviazione replicata in un server di destinazione per il test o eseguire il backup a scopo. Per altre informazioni, vedi [Domande frequenti su Replica di archiviazione](https://aka.ms/srfaq).

#### <a name="windows-admin-center-support"></a>Supporto di Windows Admin Center

Supporto per la gestione con interfaccia grafica della replica è ora disponibile in Windows Admin Center tramite lo strumento Server Manager. Ciò include la replica da server a server, la replica di cluster da cluster a cluster, nonché stretch.

#### <a name="miscellaneous-improvements"></a>Miglioramenti vari

Replica di archiviazione contiene anche i miglioramenti seguenti:

-   Modifica asincrona estensione i comportamenti di cluster in modo che i failover automatici ora avvenire
-   Più correzioni di bug

### <a name="smb"></a>SMB

- **Rimozione dell'autenticazione guest e SMB1**: Windows Server non installa più SMB1 client e server per impostazione predefinita. Inoltre, la possibilità di eseguire l'autenticazione come Guest in SMB2 e versioni successive è disattivata per impostazione predefinita. Per altre informazioni, leggi l'articolo in cui è indicato che [SMBv1 non è installato per impostazione predefinita in Windows 10 versione 1709 e Windows Server versione 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server). 

- **Compatibilità e sicurezza SMB2/SMB3**: Sono stati aggiunti ulteriori opzioni per la sicurezza e compatibilità tra applicazioni, inclusa la possibilità di disabilitare i blocchi opportunistici in SMB2 + per le applicazioni legacy, oltre a richiedere la firma o crittografia in base alla connessione da un client. Per altre informazioni, consulta la guida del modulo SMBShare PowerShell.

### <a name="data-deduplication"></a>Deduplicazione dati

- **A questo punto la deduplicazione dati supporta ReFS**: È non è più necessario scegliere tra i vantaggi di un sistema moderno file con ReFS e deduplicazione dati: a questo punto, è possibile abilitare la deduplicazione dei dati ogni volta che è possibile abilitare ReFS. Ciò comporta un aumento dell'efficienza di archiviazione di oltre il 95% con ReFS.
- **DataPort API per il traffico in ingresso/uscita ottimizzato per i volumi deduplicati**: Gli sviluppatori possono ora sfruttare le conoscenze cluster in modo efficiente e ha su come archiviare i dati in modo efficiente per spostare dati tra i volumi, i server, la deduplicazione dei dati.

### <a name="file-server-resource-manager"></a>Gestione risorse file server

Windows Server 2019 include la possibilità di impedire la creazione di un journal delle modifiche (noto anche come un journal USN) il servizio di gestione risorse File Server in tutti i volumi all'avvio del servizio. Ciò consente di risparmiare spazio su ciascun volume, ma disabilita la classificazione dei file in tempo reale. Per altre info, vedi [Panoramica di Gestione risorse file server](fsrm/fsrm-overview.md).

## <a name="whats-new-in-storage-in-windows-server-version-1803"></a>Novità nell'archiviazione in Windows Server, versione 1803

### <a name="file-server-resource-manager"></a>Gestione risorse file server

Windows Server, versione 1803 include la possibilità di impedire la creazione di un journal delle modifiche (noto anche come un journal USN) il servizio di gestione risorse File Server in tutti i volumi all'avvio del servizio. Ciò consente di risparmiare spazio su ciascun volume, ma disabilita la classificazione dei file in tempo reale. Per altre info, vedi [Panoramica di Gestione risorse file server](fsrm/fsrm-overview.md).

## <a name="whats-new-in-storage-in-windows-server-version-1709"></a>Novità nell'archiviazione in Windows Server, versione 1709

Windows Server, versione 1709 è la prima versione di Windows Server nel canale semestrale. Canale semestrale è un beneficio Software Assurance ed è pienamente supportato nell'ambiente di produzione per 18 mesi, con una nuova versione ogni sei mesi.

Per altre informazioni, vedi [Panoramica di Canale semestrale di Windows Server](../get-started/semi-annual-channel-overview.md).

### <a name="storage-replica"></a>Replica archiviazione

La protezione di ripristino di emergenza aggiunta da Replica archiviazione è stato espanso per includere:

- **Failover di test**: l'opzione per montare l'archiviazione di destinazione è ora possibile tramite la funzionalità di failover di test. È possibile montare temporaneamente uno snapshot dell'archiviazione replicata nei nodi di destinazione a scopo di test o backup. Per altre informazioni, vedi [Domande frequenti su Replica di archiviazione](https://aka.ms/srfaq).
- **Supporto di Windows Admin Center**: Supporto per la gestione con interfaccia grafica della replica è ora disponibile in Windows Admin Center tramite lo strumento Server Manager. Ciò include la replica da server a server, la replica di cluster da cluster a cluster, nonché stretch.

Replica di archiviazione contiene anche i miglioramenti seguenti:

-   Modifica asincrona estensione i comportamenti di cluster in modo che i failover automatici ora avvenire
-   Più correzioni di bug

### <a name="smb"></a>SMB

- **Rimozione dell'autenticazione guest e SMB1**: Windows Server, versione 1709 non installa più SMB1 client e server per impostazione predefinita. Inoltre, la possibilità di eseguire l'autenticazione come Guest in SMB2 e versioni successive è disattivata per impostazione predefinita. Per altre informazioni, leggi l'articolo in cui è indicato che [SMBv1 non è installato per impostazione predefinita in Windows 10 versione 1709 e Windows Server versione 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server). 

- **Compatibilità e sicurezza SMB2/SMB3**: Sono stati aggiunti ulteriori opzioni per la sicurezza e compatibilità tra applicazioni, inclusa la possibilità di disabilitare i blocchi opportunistici in SMB2 + per le applicazioni legacy, oltre a richiedere la firma o crittografia in base alla connessione da un client. Per altre informazioni, consulta la guida del modulo SMBShare PowerShell.

### <a name="data-deduplication"></a>Deduplicazione dati

- **A questo punto la deduplicazione dati supporta ReFS**: È non è più necessario scegliere tra i vantaggi di un sistema moderno file con ReFS e deduplicazione dati: a questo punto, è possibile abilitare la deduplicazione dei dati ogni volta che è possibile abilitare ReFS. Ciò comporta un aumento dell'efficienza di archiviazione di oltre il 95% con ReFS.
- **DataPort API per il traffico in ingresso/uscita ottimizzato per i volumi deduplicati**: Gli sviluppatori possono ora sfruttare le conoscenze cluster in modo efficiente e ha su come archiviare i dati in modo efficiente per spostare dati tra i volumi, i server, la deduplicazione dei dati.

## <a name="whats-new-in-storage-in-windows-server-2016"></a>Novità nell'archiviazione in Windows Server 2016

### <a name="s2d"></a>Spazi di archiviazione diretta  
Spazi di archiviazione diretta consente di creare un'archiviazione altamente disponibile e scalabile con server di archiviazione locale. Questa funzionalità semplifica la distribuzione e la gestione dei sistemi con archiviazione definita dal software e consente l'uso di nuove classi di dispositivi disco, ad esempio le unità SSD SATA e NVMe, che in precedenza non erano disponibili con spazi di archiviazione raggruppati in cluster che includevano i dischi condivisi.  

**Valore aggiunto da queste modifiche**  
Spazi di archiviazione diretta consente ai service provider e alle aziende di usare server di standard industriale con archiviazione locale per creare un'archiviazione altamente disponibile e scalabile definita dal software. L'uso di server con archiviazione locale riduce la complessità, aumenta la scalabilità e consente l'uso di dispositivi di archiviazione che non erano utilizzabili in precedenza, ad esempio i dischi a stato solido SATA per ridurre il costo dell'archiviazione flash o i dischi a stato solido NVMe per ottenere prestazioni migliori.  

Spazi di archiviazione diretta non richiede un'infrastruttura SAS condivisa e per questo semplifica i processi di distribuzione e configurazione. Essa sfrutta la rete come un'infrastruttura di archiviazione, usando SMB3 e SMB diretto (RDMA) per un'archiviazione efficiente con CPU ad alta velocità e bassa latenza. Per la scalabilità orizzontale, è sufficiente aggiungere più server per aumentare la capacità di archiviazione e le prestazioni delle operazioni di I/O  
Per altre informazioni, vedere [Storage Spaces Direct in Windows Server 2016](storage-spaces/storage-spaces-direct-overview.md) (Spazi di archiviazione diretta in Windows Server 2016).  

**Differenze di funzionamento**  
Questa funzionalità è stata introdotta in Windows Server 2016.  

### <a name="storage-replica"></a>Replica di archiviazione

Replica di archiviazione consente di eseguire la replica sincrona a livello di blocco, indipendente dall'archiviazione, tra server o cluster per il ripristino di emergenza, nonché l'estensione di un cluster di failover tra siti. La replica sincrona consente il mirroring dei dati in siti fisici con volumi coerenti per arresto anomalo del sistema senza perdere dati a livello di file system. La replica asincrona consente l'estensione del sito oltre le aree metropolitane con la possibilità di perdita di dati.  

**Valore aggiunto da queste modifiche**  
Replica archiviazione consente di eseguire le operazioni seguenti:  

* Offrire una soluzione singola di ripristino di emergenza per le interruzioni pianificate e non pianificate di carichi di lavoro di importanza critica.
* Usare il trasporto SMB3 con prestazioni, scalabilità e affidabilità comprovati.
* Estendere i cluster di failover di Windows su distanze metropolitane.
* Usare i software Microsoft end-to-end per l'archiviazione e il clustering, ad esempio Hyper-V, Replica archiviazione, Spazi di archiviazione, cluster, File server di scalabilità orizzontale, SMB3, deduplicazione e NTFS o ReFS.
* Aiuta a ridurre costi e complessità come indicato di seguito: 
    * È indipendente dall'hardware e non necessita di una configurazione di archiviazione specifica come SAN o DAS.
    * Consente le tecnologie di rete e di archiviazione.
    * Presenta una gestione grafica semplice per nodi e cluster singoli tramite Gestione cluster di failover.
    * Include le opzioni di scripting complete e su larga scala tramite Windows PowerShell. 
* Consente di ridurre i tempi di inattività e di aumentare l'affidabilità e la produttività di Windows.  
* Offre possibilità di supporto, metriche delle prestazioni e funzionalità di diagnostica.  

Per altre informazioni, vedere [Replica archiviazione in Windows Server 2016](storage-replica/storage-replica-overview.md).  

**Differenze di funzionamento**  
Questa funzionalità è stata introdotta in Windows Server 2016.  

### <a name="storage-qos"></a>QoS di archiviazione  
È ora possibile usare Qualità del servizio (QoS) di archiviazione per monitorare in modo centralizzato le prestazioni di archiviazione end-to-end e creare criteri di gestione usando cluster Hyper-V e CSV in Windows Server 2016.  

**Valore aggiunto da queste modifiche**  
È ora possibile creare criteri QoS di archiviazione in un cluster CSV e assegnarli a uno o più dischi virtuali in macchine virtuali Hyper-V. Le prestazioni di archiviazione vengono regolate automaticamente in modo da soddisfare i criteri man mano che cambiano i carichi di lavoro e di archiviazione.  

* Ogni criterio specifica una riserva (minima) e/o un limite (massimo) da applicare a una raccolta di flussi di dati, ad esempio un disco rigido virtuale, una macchina virtuale singola o un gruppo di macchine virtuali, un servizio o un tenant.  
* Usando Windows PowerShell o WMI è possibile eseguire le attività seguenti:  
    * Creare criteri in un cluster CSV.
    * Enumerare i criteri disponibili in un cluster CSV.
    * Assegnare un criterio a un disco rigido virtuale in una macchina virtuale Hyper-V. 
    * Monitorare le prestazioni di ogni flusso e lo stato all'interno del criterio.  
* Se più dischi rigidi virtuali condividono lo stesso criterio, le prestazioni vengono distribuite equamente per soddisfare la richiesta nell'intervallo del valore minimo e massimo del criterio. Pertanto, un criterio consente di gestire un disco rigido virtuale, una macchina virtuale, più macchine virtuali che comprendono un servizio o tutte le macchine virtuali di proprietà di un tenant.  

**Differenze di funzionamento**  
Questa funzionalità è stata introdotta in Windows Server 2016. La gestione delle riserve minime, il monitoraggio dei flussi di tutti i dischi virtuali nel cluster tramite un comando singolo e la gestione centralizzata basata su criteri non erano possibili nelle versioni precedenti di Windows Server.  

Per altre informazioni, vedere [QoS di archiviazione](storage-qos/storage-qos-overview.md)

### <a name="dedup"></a>Deduplicazione dei dati  
| Funzionalità | Novità o aggiornamento | Descrizione |
|---------------|----------------|-------------|
| [Supporto per volumi di grandi dimensioni](data-deduplication/whats-new.md#large-volume-support) | Aggiornamento | Prima di Windows Server 2016 i volumi dovevano essere ridimensionati in modo specifico per la varianza prevista e i volumi di dimensioni superiori a 10 TB non erano buoni candidati per la deduplicazione. In Windows Server 2016 la deduplicazione dati supporta dimensioni di volume **fino a 64 TB**. |
| [Supporto per file di grandi dimensioni](data-deduplication/whats-new.md#large-file-support) | Aggiornamento | Prima di Windows Server 2016 i file che raggiungevano 1 TB non erano buoni candidati per la deduplicazione. In Windows Server 2016 i file **fino a 1 TB** sono completamente supportati. |
| [Supporto per Nano Server](data-deduplication/whats-new.md#nano-server-support) | Nuova | La deduplicazione dati è disponibile e completamente supportata nella nuova opzione di distribuzione Nano Server per Windows Server 2016. |
| [Supporto del Backup semplificato](data-deduplication/whats-new.md#simple-backup-support) | Nuova | In Windows Server 2012 R2 le applicazioni di backup virtualizzato come [Data Protection Manager](https://technet.microsoft.com/library/hh758173.aspx) di Microsoft erano supportati grazie a una serie di passaggi di configurazione manuale. In Windows Server 2016 è stato aggiunto il nuovo tipo di utilizzo predefinito "Backup", per una distribuzione semplice della deduplicazione dati per le applicazioni di backup virtualizzato. |
| [Supporto per gli aggiornamenti in sequenza del sistema operativo Cluster](data-deduplication/whats-new.md#cluster-upgrade-support) | Nuova | La deduplicazione dati supporta la nuova funzionalità [Aggiornamento in sequenza del sistema operativo cluster](..//failover-clustering/cluster-operating-system-rolling-upgrade.md) di Windows Server 2016. |

### <a name="smb-hardening-improvements"></a>Miglioramenti per le connessioni SYSVOL e NETLOGON di protezione avanzata di SMB  
In Windows 10 e Windows Server 2016 le connessioni client alle condivisioni SYSVOL e NETLOGON predefinite di Servizi di dominio Active Directory sui controller di dominio richiedono ora la firma SMB e l'autenticazione reciproca (ad esempio Kerberos).   

**Valore aggiunto da queste modifiche**  
Questa modifica riduce il rischio di attacchi man-in-the-middle.   

**Differenze di funzionamento**  
Se la firma SMB e l'autenticazione reciproca non sono disponibili,un computer Windows 10 o Windows Server 2016 non eseguirà i criteri di gruppo e gli script basati su dominio.  

> [!NOTE]  
> I valori del Registro di sistema per queste impostazioni non sono presenti per impostazione predefinita, ma le regole di protezione avanzata si applicano comunque fino a quando non sono oggetto di override da criteri di gruppo o altri valori del Registro di sistema.  

Per altre informazioni su questi miglioramenti di sicurezza - noto anche per come protezione avanzata UNC, vedere l'articolo della Microsoft Knowledge Base [3000483](https://support.microsoft.com/kb/3000483) e [MS15 011 & MS15 014: Criteri di gruppo di protezione avanzata](https://blogs.technet.microsoft.com/srd/2015/02/10/ms15-011-ms15-014-hardening-group-policy).  

### <a name="work-folders"></a>Cartelle di lavoro
Notifica della modifica migliorata quando il server di cartelle di lavoro è in esecuzione Windows Server 2016 e il client di cartelle di lavoro è Windows 10.

**Valore aggiunto da queste modifiche**<br>
Con Windows Server 2012 R2, quando le modifiche ai file vengono sincronizzate con il server di Cartelle di lavoro, i client non ricevono una notifica delle modifiche e l'aggiornamento può arrivare anche 10 minuti dopo.  Quando si usa Windows Server 2016, il server di cartelle di lavoro immediatamente notifica ai client di Windows 10 e i file vengono sincronizzate immediatamente.

**Differenze di funzionamento**<br>
Questa funzionalità è stata introdotta in Windows Server 2016. Richiede un server Cartelle di lavoro con Windows Server 2016 e il client deve essere Windows 10.

Se si usa un client precedente o il server Cartelle di lavoro è Windows Server 2012 R2, il client continuerà a eseguire il polling delle modifiche ogni 10 minuti.

### <a name="refs"></a>ReFS 
La successiva iterazione di ReFS offre il supporto per distribuzioni di archiviazione su larga scala, con carichi di lavoro diversificati, al fine di fornire affidabilità, resilienza e scalabilità per i dati.     

**Valore aggiunto da queste modifiche**<br>
ReFS introduce i seguenti miglioramenti:

* ReFS implementa la nuova funzionalità di livelli di archiviazione, consentendo così prestazioni più rapide e maggiore capacità di archiviazione. La nuova funzionalità offre le seguenti caratteristiche:
    * Più tipi di resilienza sullo stesso disco virtuale (con la possibilità di utilizzare il mirroring nel livello delle prestazioni e la parità nel livello della capacità, ad esempio).
    * Maggiore velocità di risposta ai working set di deriva.  
* L'introduzione della clonazione dei blocchi migliora in modo sostanziale le prestazioni delle operazioni VM, ad esempio le operazioni di unione del checkpoint .vhdx.
* Il nuovo strumento di analisi ReFS consente di ripristinare una risorsa di archiviazione persa e di recuperare i dati da danneggiamenti critici. 

**Differenze di funzionamento**<br>
Queste funzionalità sono nuove in Windows Server 2016. 

## <a name="see-also"></a>Vedere anche  
* [Novità di Windows Server 2016](../get-started/what-s-new-in-windows-server-2016.md)  
