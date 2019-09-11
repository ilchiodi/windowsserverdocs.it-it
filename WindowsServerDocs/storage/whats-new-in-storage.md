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
ms.openlocfilehash: 3548d6c239c81deb50ce698767db5eb287046f17
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867042"
---
# <a name="whats-new-in-storage-in-windows-server"></a>Novità di archiviazione in Windows Server

>Si applica a Windows Server 2019, Windows Server 2016, Windows Server (Canale semestrale)

In questo argomento vengono illustrate le funzionalità nuove e modificate di archiviazione in Windows Server 2019, Windows Server 2016 e versioni di canale semestrali di Windows Server.

## <a name="whats-new-in-storage-in-windows-server-version-1903"></a>Novità di archiviazione in Windows Server, versione 1903

Questa versione di Windows Server aggiunge le modifiche e le tecnologie seguenti.

### <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>Il servizio di migrazione archiviazione esegue ora la migrazione di account locali, cluster e server Linux

Il servizio di migrazione archiviazione semplifica la migrazione dei server a una versione più recente di Windows Server. Fornisce uno strumento grafico che archivia i dati nei server, quindi trasferisce i dati e la configurazione in server più recenti, tutto senza che le app o gli utenti debbano apportare alcuna modifica.

Per l'uso di questa versione di Windows Server per orchestrare le migrazioni, sono state aggiunte le funzionalità seguenti:

- Migrazione di utenti e gruppi locali al nuovo server
- Migrazione degli archivi dai cluster di failover
- Migrazione degli archivi da un server Linux che usa Samba
- Sincronizzazione semplificata tramite Sincronizzazione file di Azure delle condivisioni sottoposte a migrazione
- Migrazione a nuove reti, ad esempio Azure

Per altre informazioni sul servizio di migrazione archiviazione, vedi [Panoramica del servizio di migrazione archiviazione](storage-migration-service/overview.md).

### <a name="system-insights-disk-anomaly-detection"></a>Rilevamento delle anomalie dei dischi tramite informazioni dettagliate di sistema

[Informazioni dettagliate di sistema](../manage/system-insights/overview.md) è una funzionalità analitica predittiva che analizza i dati di sistema di Windows Server in locale e offre informazioni approfondite sul funzionamento del server. È dotata di numerose funzionalità incorporate, ma abbiamo aggiunto la possibilità di installare funzionalità aggiuntive tramite Windows Admin Center, a partire dal rilevamento delle anomalie dei dischi.

Il rilevamento delle anomalie dei dischi è una nuova funzionalità che indica quando i dischi si comportano *in modo diverso* rispetto alla normalità. Anche se un comportamento diverso non ha necessariamente una connotazione negativa, può essere utile esaminare questi momenti di anomalia nel corso della risoluzione dei problemi dei sistemi.

Questa funzionalità è disponibile anche per i server che eseguono Windows Server 2019.

### <a name="windows-admin-center-enhancements"></a>Miglioramenti di Windows Admin Center

È disponibile una nuova versione di Windows Admin Center, che aggiunge nuove funzionalità a Windows Server. Per informazioni sulle funzionalità più recenti, vedi [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md).

## <a name="whats-new-in-storage-in-windows-server-2019-and-windows-server-version-1809"></a>Novità di archiviazione in Windows Server 2019 e Windows Server, versione 1809

Questa versione di Windows Server aggiunge le modifiche e le tecnologie seguenti.

### <a name="manage-storage-with-windows-admin-center"></a>Gestire l'archiviazione con l'interfaccia di amministrazione di Windows

[Centro di amministrazione di Windows](../manage/windows-admin-center/overview.md) è una nuova app basata su browser distribuita localmente per la gestione di server, cluster, infrastruttura iperconvergente con spazi di archiviazione diretta e PC Windows 10. Non è disponibile alcun costo aggiuntivo oltre a Windows ed è pronto per l'uso in produzione.

Per essere onesti, l'interfaccia di amministrazione di Windows è un download separato eseguito in Windows Server 2019 e altre versioni di Windows, ma è nuovo e non è stato possibile perderlo...

### <a name="storage-migration-service"></a>Servizio di migrazione della risorsa di archiviazione

Il servizio di migrazione archiviazione è una nuova tecnologia che semplifica la migrazione dei server a una versione più recente di Windows Server. Fornisce uno strumento grafico che esegue l'inventario dei dati nei server, trasferisce i dati e la configurazione nei server più recenti e, facoltativamente, sposta le identità dei server precedenti ai nuovi server in modo che le app e gli utenti non debbano apportare alcuna modifica. Per altre info, vedi [Servizio di migrazione della risorsa di archiviazione](storage-migration-service/overview.md).

### <a id="storage-spaces-direct"></a>Spazi di archiviazione diretta (solo Windows Server 2019)

Sono stati apportati numerosi miglioramenti Spazi di archiviazione diretta in Windows Server 2019 (Spazi di archiviazione diretta non è incluso in Windows Server, un canale semestrale):

- **Deduplicazione e compressione dei volumi ReFS**

    Archivia fino a dieci volte più dati nello stesso volume con deduplicazione e compressione per il file System ReFS. È [sufficiente un solo clic](https://www.youtube.com/watch?v=PRibTacyKko&feature=youtu.be) per attivare l'interfaccia di amministrazione di Windows. L'archivio di blocchi di dimensioni variabili con compressione facoltativa ottimizza i tassi di risparmio, mentre l'architettura di post-elaborazione multithread mantiene un effetto minimo sulle prestazioni. Supporta volumi fino a 64 TB e deduplica i primi 4 TB di ogni file.

- **Supporto nativo per la memoria persistente**

    Usufruisci di prestazioni senza precedenti con il supporto di Spazi di archiviazione diretta in modalità nativa per moduli di memoria persistente, inclusi Intel® Optane™ DC PM e NVDIMM-n. Usa la memoria persistente come cache per accelerare il working set attivo o come capacità per garantire bassa latenza coerente nell'ordine di microsecondi. Gestisci la memoria persistente esattamente come faresti per qualsiasi altra unità in PowerShell o Windows Admin Center.

- **Resilienza annidata per l'infrastruttura iperconvergente a due nodi periferica**

    Supera due errori hardware alla volta con un'opzione di resilienza software completamente nuova ispirata a RAID 5+1. Con la resilienza nidificata, un cluster di Spazi di archiviazione diretta a due nodi può fornire un archivio sempre accessibile per le app e le macchine virtuali anche se un nodo server diventa inattivo e un'unità diventa non disponibile nell'altro nodo server.

- **Cluster a due server con un'unità flash USB di controllo**

    Usare un'unità flash USB a basso costo collegata al router per fungere da server di controllo del mirroring in cluster con due server. Se un server viene arrestato e quindi sottoposto a backup, il cluster di unità USB sa quale server dispone dei dati più aggiornati. Per altre informazioni, vedere il [Blog relativo all'archiviazione in Microsoft](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/).

- **Windows Admin Center**

    Gestisci e monitora Spazi di archiviazione diretta con il nuovo [dashboard specificatamente progettato](../manage/windows-admin-center/use/manage-hyper-converged.md) e l'esperienza in Windows Admin Center. Crea, apri, espandi o elimina volumi con pochi clic. Monitora prestazioni, come la latenza I/O e IOPS dall'intero cluster fino al singolo SSD o HDD. Disponibile senza alcun costo aggiuntivo per Windows Server 2016 e Windows Server 2019.

- **Cronologia delle prestazioni**

    Ottieni facilmente visibilità sull'utilizzo delle risorse e sulle prestazioni con [cronologia predefinita](storage-spaces/performance-history.md). I valori di oltre 50 contatori essenziali, ad esempio relativi a elaborazione, memoria, rete e archiviazione, vengono raccolti e archiviati automaticamente nel cluster fino a un anno. Meglio, non c'è niente da installare, configurare o avviare, ma funziona. Visualizza in Windows Admin Center o esegui query ed elabora i risultati in PowerShell.

- **Scalabilità fino a 4 PB per ogni cluster**

    Ottieni un ridimensionamento di più petabyte per supporti di memorizzazione, backup e archiviazione. In Windows Server 2019, Spazi di archiviazione diretta supporta fino a 4 petabyte (PB) = 4.000 TB di capacità non elaborata per pool di archiviazione. Anche le linee guida correlate alla capacità sono maggiori: ad esempio, puoi creare il doppio dei volumi (64 anziché 32), ognuno due volte più grande di prima (64 TB anziché 32 TB). Unire più cluster in un [cluster impostato](storage-spaces/cluster-sets.md) per una scalabilità ancora maggiore all'interno di uno spazio dei nomi di archiviazione. Per altre informazioni, vedere il [Blog relativo all'archiviazione in Microsoft](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/).

- **Parità accelerata con mirroring 2 volte più veloce**

    Mediante la parità accelerata con mirroring puoi creare volumi con Spazi di archiviazione diretta in grado di offrire in parte mirroring e in parte parità, ad esempio puoi unire RAID-1 e RAID-5/6 per ottenere il meglio di entrambi. È [più semplice di quanto si pensi](https://www.youtube.com/watch?v=R72QHudqWpE) nell'interfaccia di amministrazione di Windows. In Windows Server 2019, le prestazioni della parità con accelerazione speculare sono superiori rispetto al doppio rispetto a Windows Server 2016 grazie alle ottimizzazioni.

- **Rilevamento dell'outlier con latenza unità**

    Identifica con facilità le unità con latenza anomala grazie al monitoraggio proattivo e al rilevamento di outlier incorporato, ispirato dall'approccio di Microsoft Azure di lunga durata e di successo. Sia che si tratti di una latenza media o di un elemento più sottile, ad esempio la latenza 99 ° percentile che emerge, le unità lente vengono automaticamente etichettate in PowerShell e nell'interfaccia di amministrazione di Windows con lo stato di latenza

- **Delimitazione manuale dell'allocazione dei volumi per aumentare la tolleranza di errore**

    Ciò consente agli amministratori di delimitare manualmente l'allocazione di volumi in Spazi di archiviazione diretta. Questa operazione può aumentare significativamente la tolleranza di errore in determinate condizioni, ma impone alcune considerazioni e complessità di gestione aggiuntive. Per altre informazioni, vedere [delimitare l'allocazione dei volumi](storage-spaces/delimit-volume-allocation.md).

### <a name="storage-replica2019"></a>Replica archiviazione

In questa versione sono stati apportati numerosi miglioramenti alla [replica di archiviazione](storage-replica/storage-replica-overview.md) :

#### <a name="storage-replica-in-windows-server-standard-edition"></a>Replica di archiviazione in Windows Server, Standard Edition

È ora possibile usare replica archiviazione con Windows Server, Standard Edition, oltre a Datacenter Edition. La replica di archiviazione in esecuzione in Windows Server, Standard Edition, presenta le limitazioni seguenti:

- Replica archiviazione replica un singolo volume anziché un numero illimitato di volumi.
- I volumi possono avere dimensioni fino a 2 TB anziché una dimensione illimitata.

#### <a name="storage-replica-log-performance-improvements"></a>Miglioramenti delle prestazioni del log per Replica di archiviazione

Sono stati apportati miglioramenti anche al modo in cui il log delle repliche di archiviazione tiene traccia della replica, migliorando la velocità effettiva e la latenza della replica, in particolare nell'archiviazione di tutti i flash e Spazi di archiviazione diretta cluster che si replicano tra loro

Per ottenere prestazioni elevate, tutti i membri del gruppo di replica devono eseguire Windows Server 2019.

#### <a name="test-failover"></a>Failover di test

È ora possibile montare temporaneamente uno snapshot dell'archiviazione replicata in un server di destinazione a scopo di test o di backup. Per altre informazioni, vedi [Domande frequenti su Replica di archiviazione](https://aka.ms/srfaq).

#### <a name="windows-admin-center-support"></a>Supporto di Windows Admin Center

Il supporto per la gestione grafica della replica è ora disponibile nel centro di amministrazione di Windows tramite lo strumento Server Manager. Sono incluse la replica da server a server, da cluster a cluster, nonché la replica del cluster esteso.

#### <a name="miscellaneous-improvements"></a>Miglioramenti vari

Replica archiviazione contiene inoltre i miglioramenti seguenti:

-   Modifica i comportamenti asincroni del cluster esteso in modo che ora si verifichino failover automatici
-   Più correzioni di bug

### <a name="smb"></a>SMB

- **Rimozione dell'autenticazione guest e SMB1**: Per impostazione predefinita, Windows Server non installa più il client e il server di SMB1. Inoltre, la possibilità di eseguire l'autenticazione come Guest in SMB2 e versioni successive è disattivata per impostazione predefinita. Per altre informazioni, leggere l'articolo in cui è indicato che [SMBv1 non è installato per impostazione predefinita in Windows 10 versione 1709 e Windows Server versione 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server). 

- **Compatibilità e sicurezza SMB2/SMB3**: sono state aggiunte ulteriori opzioni per la compatibilità delle applicazioni e la sicurezza, inclusa la possibilità di disabilitare oplock in SMB2+ per le applicazioni legacy e di richiedere la firma o codifica per ogni connessione da un client. Per altre informazioni, consulta la guida del modulo SMBShare PowerShell.

### <a name="data-deduplication"></a>Deduplicazione dati

- **Deduplicazione dati supporta ora ReFS**: non è più necessario scegliere tra i vantaggi di un file system moderno con ReFS e Deduplicazione dati. È ora possibile abilitare Deduplicazione dati ovunque sia possibile abilitare ReFS. Ciò comporta un aumento dell'efficienza di archiviazione di oltre il 95% con ReFS.
- **API DataPort per traffico in ingresso/uscita ottimizzato ai volumi deduplicati**: gli sviluppatori possono ora sfruttare le informazioni di cui dispone Deduplicazione dati su come archiviare dati in modo efficiente per spostarli tra server, volumi e cluster in modo efficiente.

### <a name="file-server-resource-manager"></a>Gestione risorse file server

Windows Server 2019 include la possibilità di impedire al file server Gestione risorse servizio di creare un journal delle modifiche (noto anche come journal USN) su tutti i volumi all'avvio del servizio. Ciò consente di risparmiare spazio su ciascun volume, ma disabilita la classificazione dei file in tempo reale. Per altre info, vedi [Panoramica di Gestione risorse file server](fsrm/fsrm-overview.md).

## <a name="whats-new-in-storage-in-windows-server-version-1803"></a>Novità di archiviazione in Windows Server, versione 1803

### <a name="file-server-resource-manager"></a>Gestione risorse file server

Windows Server versione 1803 include la possibilità di impedire al servizio file server Gestione risorse di creare un journal delle modifiche (noto anche come journal USN) su tutti i volumi all'avvio del servizio. Ciò consente di risparmiare spazio su ciascun volume, ma disabilita la classificazione dei file in tempo reale. Per altre info, vedi [Panoramica di Gestione risorse file server](fsrm/fsrm-overview.md).

## <a name="whats-new-in-storage-in-windows-server-version-1709"></a>Novità di archiviazione in Windows Server, versione 1709

Windows Server, versione 1709 è la prima versione di Windows Server nel canale semestrale. Il canale semestrale è un vantaggio Software Assurance ed è completamente supportato nell'ambiente di produzione per 18 mesi, con una nuova versione ogni sei mesi.

Per altre informazioni, vedi [Panoramica di Canale semestrale di Windows Server](../get-started/semi-annual-channel-overview.md).

### <a name="storage-replica"></a>Replica archiviazione

La protezione del ripristino di emergenza aggiunta da replica archiviazione è ora espansa per includere:

- **Failover di test**: l'opzione per montare l'archiviazione di destinazione è ora possibile tramite la funzionalità di failover di test. È possibile montare temporaneamente uno snapshot dell'archiviazione replicata nei nodi di destinazione a scopo di test o backup. Per altre informazioni, vedi [Domande frequenti su Replica di archiviazione](https://aka.ms/srfaq).
- **Supporto**dell'interfaccia di amministrazione di Windows: Il supporto per la gestione grafica della replica è ora disponibile nel centro di amministrazione di Windows tramite lo strumento Server Manager. Sono incluse la replica da server a server, da cluster a cluster, nonché la replica del cluster esteso.

Replica archiviazione contiene inoltre i miglioramenti seguenti:

-   Modifica i comportamenti asincroni del cluster esteso in modo che ora si verifichino failover automatici
-   Più correzioni di bug

### <a name="smb"></a>SMB

- **Rimozione dell'autenticazione guest e SMB1**: Windows Server, versione 1709 non esegue più l'installazione del server e del client SMB1 per impostazione predefinita. Inoltre, la possibilità di eseguire l'autenticazione come Guest in SMB2 e versioni successive è disattivata per impostazione predefinita. Per altre informazioni, leggere l'articolo in cui è indicato che [SMBv1 non è installato per impostazione predefinita in Windows 10 versione 1709 e Windows Server versione 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server). 

- **Compatibilità e sicurezza SMB2/SMB3**: sono state aggiunte ulteriori opzioni per la compatibilità delle applicazioni e la sicurezza, inclusa la possibilità di disabilitare oplock in SMB2+ per le applicazioni legacy e di richiedere la firma o codifica per ogni connessione da un client. Per altre informazioni, consulta la guida del modulo SMBShare PowerShell.

### <a name="data-deduplication"></a>Deduplicazione dati

- **Deduplicazione dati supporta ora ReFS**: non è più necessario scegliere tra i vantaggi di un file system moderno con ReFS e Deduplicazione dati. È ora possibile abilitare Deduplicazione dati ovunque sia possibile abilitare ReFS. Ciò comporta un aumento dell'efficienza di archiviazione di oltre il 95% con ReFS.
- **API DataPort per traffico in ingresso/uscita ottimizzato ai volumi deduplicati**: gli sviluppatori possono ora sfruttare le informazioni di cui dispone Deduplicazione dati su come archiviare dati in modo efficiente per spostarli tra server, volumi e cluster in modo efficiente.

## <a name="whats-new-in-storage-in-windows-server-2016"></a>Novità nell'archiviazione in Windows Server 2016

### <a name="s2d"></a>Spazi di archiviazione diretta  
Spazi di archiviazione diretta consente di creare un'archiviazione altamente disponibile e scalabile con server di archiviazione locale. Questa funzionalità semplifica la distribuzione e la gestione dei sistemi con archiviazione definita dal software e consente l'uso di nuove classi di dispositivi disco, ad esempio le unità SSD SATA e NVMe, che in precedenza non erano disponibili con spazi di archiviazione raggruppati in cluster che includevano i dischi condivisi.  

**Valore aggiunto da queste modifiche**  
Spazi di archiviazione diretta consente ai service provider e alle aziende di usare server di standard industriale con archiviazione locale per creare un'archiviazione altamente disponibile e scalabile definita dal software. L'uso di server con archiviazione locale riduce la complessità, aumenta la scalabilità e consente l'uso di dispositivi di archiviazione che non erano utilizzabili in precedenza, ad esempio i dischi a stato solido SATA per ridurre il costo dell'archiviazione flash o i dischi a stato solido NVMe per ottenere prestazioni migliori.  

Spazi di archiviazione diretta non richiede un'infrastruttura SAS condivisa e per questo semplifica i processi di distribuzione e configurazione. Essa sfrutta la rete come un'infrastruttura di archiviazione, usando SMB3 e SMB diretto (RDMA) per un'archiviazione efficiente con CPU ad alta velocità e bassa latenza. Per la scalabilità orizzontale, è sufficiente aggiungere più server per aumentare la capacità di archiviazione e le prestazioni delle operazioni di I/O  
Per altre informazioni, vedere [Storage Spaces Direct in Windows Server 2016](storage-spaces/storage-spaces-direct-overview.md) (Spazi di archiviazione diretta in Windows Server 2016).  

**Differenze di funzionamento**  
Questa funzionalità è stata introdotta in Windows Server 2016.  

### <a name="storage-replica"></a>Replica archiviazione

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

### <a name="storage-qos"></a>Qualità del servizio di archiviazione  
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

### <a name="dedup"></a>Deduplicazione dati  
| Funzionalità | Novità o aggiornamento | Descrizione |
|---------------|----------------|-------------|
| [Supporto per volumi di grandi dimensioni](data-deduplication/whats-new.md#large-volume-support) | Aggiornamento | Prima di Windows Server 2016 i volumi dovevano essere ridimensionati in modo specifico per la varianza prevista e i volumi di dimensioni superiori a 10 TB non erano buoni candidati per la deduplicazione. In Windows Server 2016 la deduplicazione dati supporta dimensioni di volume **fino a 64 TB**. |
| [Supporto per file di grandi dimensioni](data-deduplication/whats-new.md#large-file-support) | Aggiornamento | Prima di Windows Server 2016 i file che raggiungevano 1 TB non erano buoni candidati per la deduplicazione. In Windows Server 2016 i file **fino a 1 TB** sono completamente supportati. |
| [Supporto per nano server](data-deduplication/whats-new.md#nano-server-support) | Nuova | La deduplicazione dati è disponibile e completamente supportata nella nuova opzione di distribuzione Nano Server per Windows Server 2016. |
| [Supporto semplificato per il backup](data-deduplication/whats-new.md#simple-backup-support) | Nuova | In Windows Server 2012 R2 le applicazioni di backup virtualizzato come [Data Protection Manager](https://technet.microsoft.com/library/hh758173.aspx) di Microsoft erano supportati grazie a una serie di passaggi di configurazione manuale. In Windows Server 2016 è stato aggiunto il nuovo tipo di utilizzo predefinito "Backup", per una distribuzione semplice della deduplicazione dati per le applicazioni di backup virtualizzato. |
| [Supporto per gli aggiornamenti in sequenza del sistema operativo del cluster](data-deduplication/whats-new.md#cluster-upgrade-support) | Nuova | La deduplicazione dati supporta la nuova funzionalità [Aggiornamento in sequenza del sistema operativo cluster](..//failover-clustering/cluster-operating-system-rolling-upgrade.md) di Windows Server 2016. |

### <a name="smb-hardening-improvements"></a>Miglioramenti della protezione avanzata SMB per le connessioni SYSVOL e NETLOGON  
In Windows 10 e Windows Server 2016 le connessioni client alle condivisioni SYSVOL e NETLOGON predefinite di Servizi di dominio Active Directory sui controller di dominio richiedono ora la firma SMB e l'autenticazione reciproca (ad esempio Kerberos).   

**Valore aggiunto da queste modifiche**  
Questa modifica riduce il rischio di attacchi man-in-the-middle.   

**Differenze di funzionamento**  
Se la firma SMB e l'autenticazione reciproca non sono disponibili,un computer Windows 10 o Windows Server 2016 non eseguirà i criteri di gruppo e gli script basati su dominio.  

> [!NOTE]  
> I valori del Registro di sistema per queste impostazioni non sono presenti per impostazione predefinita, ma le regole di protezione avanzata si applicano comunque fino a quando non sono oggetto di override da criteri di gruppo o altri valori del Registro di sistema.  

Per ulteriori informazioni sui miglioramenti apportati alla sicurezza, noti anche come protezione avanzata UNC, vedere l'articolo [3000483](https://support.microsoft.com/kb/3000483) della [Microsoft Knowledge base e MS15-011 & MS15-014: Protezione avanzata Criteri di gruppo](https://blogs.technet.microsoft.com/srd/2015/02/10/ms15-011-ms15-014-hardening-group-policy).  

### <a name="work-folders"></a>Cartelle di lavoro
Notifica di modifica migliorata quando il server di cartelle di lavoro esegue Windows Server 2016 e il client di cartelle di lavoro è Windows 10.

**Valore aggiunto da queste modifiche**<br>
Con Windows Server 2012 R2, quando le modifiche ai file vengono sincronizzate con il server di Cartelle di lavoro, i client non ricevono una notifica delle modifiche e l'aggiornamento può arrivare anche 10 minuti dopo.  Quando si usa Windows Server 2016, il server di cartelle di lavoro invia immediatamente una notifica ai client Windows 10 e le modifiche apportate ai file vengono sincronizzate immediatamente.

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
