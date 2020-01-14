---
title: Panoramica della condivisione di file con il protocollo SMB 3 in Windows Server
description: Panoramica dell'uso del protocollo SMB 3 per le condivisioni file e il servizio file con Windows Server.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 01/10/2020
ms.localizationpriority: medium
ms.openlocfilehash: 416145a8c4ec20eaf46cf4b5ac88a0cdf38bdf33
ms.sourcegitcommit: 76469d1b7465800315eaca3e0c7f0438fc3939ed
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2020
ms.locfileid: "75919889"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>Panoramica della condivisione di file con il protocollo SMB 3 in Windows Server

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento descrive la funzionalità SMB 3 di Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012: usi pratici per la funzionalità, le funzionalità nuove o aggiornate più significative di questa versione rispetto alle versioni precedenti e requisiti hardware. SMB è anche un protocollo di infrastruttura usato dalle soluzioni [SDDC (software-defined Data Center)](../../sddc.md) , ad esempio spazi di archiviazione diretta, replica di archiviazione e altro ancora. La versione 3,0 di SMB è stata introdotta con Windows Server 2012 ed è stata migliorata in modo incrementale nelle versioni successive.

## <a name="feature-description"></a>Descrizione delle caratteristiche

SMB (Server Message Block) è un protocollo di condivisione di file di rete che consente alle applicazioni in un computer di leggere e scrivere da/su file, nonché di richiedere servizi da programmi server in una rete di computer. Il protocollo SMB può essere usato in aggiunta al proprio protocollo TCP/IP o ad altri protocolli di rete. Usando il protocollo SMB, un'applicazione (o l'utente di un'applicazione) può accedere a file o ad altre risorse presso un server remoto. Ciò consente alle applicazioni di leggere, creare e aggiornare file nel server remoto. SMB è inoltre in grado di comunicare con qualsiasi programma server configurato per ricevere una richiesta client SMB. SMB è un protocollo di infrastruttura usato dalle tecnologie di calcolo SDDC (software-defined Data Center), ad esempio Spazi di archiviazione diretta, replica di archiviazione. Per ulteriori informazioni, vedere [data center definito dal software di Windows Server](../../sddc.md).

## <a name="practical-applications"></a>Applicazioni pratiche

Questa sezione illustra alcuni nuovi metodi pratici per usare il nuovo protocollo SMB 3.0.

* **Archiviazione di file per la virtualizzazione (Hyper-V ™ su SMB)** . Hyper-V può archiviare i file di una macchina virtuale, i file di configurazione, i file del disco rigido virtuale (VHD) e gli snapshot nelle condivisioni file SMB 3.0. Questa funzionalità può essere usata per file server autonomi e file server in cluster che usano Hyper-V con archiviazione di file condivisi per il cluster.
* **Microsoft SQL Server su SMB**. SQL Server può archiviare file del database utente nelle condivisioni file SMB. Attualmente questa funzionalità è supportata con SQL Server 2008 R2 per i server SQL autonomi. Nelle prossime versioni di SQL Server verrà aggiunto il supporto per SQL Server in cluster e database di sistema.
* **Archiviazione tradizionale per i dati dell'utente finale**. Il protocollo SMB 3.0 fornisce miglioramenti per i carichi di lavoro degli Information Worker. Questi miglioramenti includono la riduzione delle latenze di applicazione riscontrate dagli utenti delle succursali durante l'accesso ai dati su una rete WAN (Wide Area Network) e la protezione dei dati da attacchi di intercettazione.

## <a name="new-and-changed-functionality"></a>Funzionalità nuove e modificate

Le sezioni seguenti descrivono le funzionalità che sono state aggiunte in SMB 3 e gli aggiornamenti successivi.

## <a name="features-added-in-windows-server-2019-and-windows-10-version-1809"></a>Funzionalità aggiunte in Windows Server 2019 e Windows 10, versione 1809

| Caratteristica/funzionalità  | Novità o aggiornamento  | Riepilogo  |
| --------- | --------- | --------- |
| Possibilità di richiedere la scrittura su disco su condivisioni file che non sono costantemente disponibili | Nuova | Per fornire una garanzia aggiuntiva che consente di scrivere in una condivisione file fino al passaggio dello stack software e hardware al disco fisico prima che l'operazione di scrittura restituisca il risultato come completato, è possibile abilitare il write-through nella condivisione file usando il `NET USE /WRITETHROUGH` comando ore il `New-SMBMapping -UseWriteThrough` cmdlet di PowerShell. L'uso di Write-through presenta una certa quantità di prestazioni; Per ulteriori informazioni, vedere il post di Blog relativo al [controllo dei comportamenti write-through in SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-write-through-behaviors-in-smb/bc-p/1083417#M677) . |

## <a name="features-added-in-windows-server-version-1709-and-windows-10-version-1709"></a>Funzionalità aggiunte in Windows Server, versione 1709 e Windows 10, versione 1709

| Caratteristica/funzionalità  | Novità o aggiornamento  | Riepilogo  |
| --------- | --------- | --------- |
| L'accesso guest alle condivisioni file è disabilitato | Nuova | Il client SMB non consente più di eseguire le azioni seguenti: accesso dell'account Guest a un server remoto; Viene eseguito il fallback all'account Guest dopo che sono state specificate credenziali non valide. Per informazioni dettagliate, vedere [accesso guest in SMB2 disabilitato per impostazione predefinita in Windows](https://support.microsoft.com/help/4046019/guest-access-in-smb2-disabled-by-default-in-windows-10-and-windows-ser). | 
| Mapping globale SMB | Nuova | Esegue il mapping di una condivisione SMB remota a una lettera di unità accessibile a tutti gli utenti nell'host locale, inclusi i contenitori. Questa operazione è necessaria per abilitare l'I/O del contenitore nel volume di dati per attraversare il punto di montaggio remoto. Tenere presente che quando si usa il mapping globale SMB per i contenitori, tutti gli utenti nell'host del contenitore possono accedere alla condivisione remota. Tutte le applicazioni in esecuzione nell'host del contenitore possono anche accedere alla condivisione remota mappata. Per informazioni dettagliate, vedere Supporto per l' [archiviazione di contenitori con volumi condivisi cluster (CSV), spazi di archiviazione diretta, mapping globale SMB](https://techcommunity.microsoft.com/t5/failover-clustering/container-storage-support-with-cluster-shared-volumes-csv/ba-p/372140). |
| Controllo dialetto SMB | Nuova | È ora possibile impostare i valori del registro di sistema per controllare la versione minima SMB (dialetto) e la versione massima SMB usata. Per informazioni dettagliate, vedere [controllo dei dialetti SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024). |

## <a name="features-added-in-smb-311-with-windows-server-2016-and-windows-10-version-1607"></a>Funzionalità aggiunte in SMB 3,11 con Windows Server 2016 e Windows 10, versione 1607

| Caratteristica/funzionalità  | Novità o aggiornamento  | Riepilogo  |
| --------- | --------- | --------- |
| Crittografia SMB     |   Aggiornamento      | La crittografia SMB 3.1.1 con Advanced Encryption Standard-Galois/Counter Mode (AES-GCM) è più veloce della firma SMB o della crittografia SMB precedente con AES-CCM.   |
| Caching della directory | Nuova | SMB 3.1.1 include miglioramenti alla memorizzazione nella cache delle directory. I client Windows possono ora memorizzare nella cache directory molto più grandi, approssimativamente 500.000 voci. I client Windows tenterà di eseguire query di directory con buffer da 1 MB per ridurre i round trip e migliorare le prestazioni. |
| Integrità pre-autenticazione | Nuova |  In SMB 3.1.1 l'integrità di pre-autenticazione offre una migliore protezione da un utente malintenzionato che si occupa di un attacco man-in-the-Middle manomettendo i messaggi di autenticazione e creazione della connessione di SMB. Per informazioni dettagliate, vedere [l'integrità di pre-autenticazione SMB 3.1.1 in Windows 10](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10). |
| Miglioramenti della crittografia SMB | Nuova | SMB 3.1.1 offre un meccanismo per negoziare l'algoritmo di crittografia per connessione, con le opzioni per AES-128-CCM e AES-128-GCM. AES-128-GCM è l'impostazione predefinita per le nuove versioni di Windows, mentre le versioni precedenti continueranno a usare AES-128-CCM. |
| Supporto dell'aggiornamento cluster in sequenza | Nuova | Consente di eseguire gli [aggiornamenti in sequenza del cluster](../../failover-clustering/cluster-operating-system-rolling-upgrade.md) consentendo a SMB di supportare versioni massime diverse di SMB per i cluster in fase di aggiornamento. Per informazioni dettagliate su come consentire la comunicazione SMB usando versioni diverse (dialetti) del protocollo, vedere il post di Blog relativo al [controllo dei dialetti SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024). |
| Supporto client diretto SMB in Windows 10 | Nuova | Windows 10 Enterprise, Windows 10 Education e Windows 10 Pro per le workstation ora includono il supporto client diretto SMB. |
| Supporto nativo per le chiamate API FileNormalizedNameInformation | Nuova | Aggiunge il supporto nativo per l'esecuzione di query sul nome normalizzato di un file. Per informazioni dettagliate, vedere [FileNormalizedNameInformation](https://docs.microsoft.com/openspecs/windows_protocols/ms-fscc/20bcadba-808c-4880-b757-4af93e41edf6). |

Per ulteriori informazioni, vedere il post di Blog sulle [novità di SMB 3.1.1 in Windows Server 2016 Technical Preview 2](https://docs.microsoft.com/archive/blogs/josebda/whats-new-in-smb-3-1-1-in-the-windows-server-2016-technical-preview-2).

## <a name="features-added-in-smb-302-with-windows-server-2012-r2-and-windows-81"></a>Funzionalità aggiunte in SMB 3,02 con Windows Server 2012 R2 e Windows 8.1

| Caratteristica/funzionalità  | Novità o aggiornamento  | Riepilogo  |
| --------- | --------- | --------- |
| Ribilanciamento automatico dei client di file server di scalabilità orizzontale     |   Nuova      | Migliora la scalabilità e la gestibilità per i file server di scalabilità orizzontale. Le connessioni client SMB vengono registrate per ogni condivisione file, anziché per ogni server, e i client vengono quindi reindirizzati al nodo del cluster con l'accesso migliore al volume utilizzato dalla condivisione file. In questo modo si ottiene un miglioramento dell'efficienza grazie alla riduzione del reindirizzamento del traffico tra i nodi del file server. I client vengono reindirizzati in seguito alla connessione iniziale e alla riconfigurazione dell'archiviazione del cluster.    |
| Prestazioni sulla rete WAN   | Aggiornamento  | Windows 8.1 e Windows 10 forniscono un miglioramento della SRV_COPYCHUNK CopyFile rispetto al supporto SMB quando si usa Esplora file per le copie remote da un percorso in un computer remoto a un'altra copia nello stesso server. Si copierà solo una piccola quantità di metadati sulla rete (1/2KiB per 16MiB di dati di file vengono trasmessi). Questo comporta un miglioramento significativo delle prestazioni. Si tratta di una distinzione a livello di sistema operativo e di Esplora file per SMB. |
| SMB diretto     |   Aggiornamento      | Le prestazioni per i carichi di lavoro di I/O ridotti risultano migliorate grazie a una maggiore efficienza per l'hosting dei carichi di lavoro con operazioni di I/O limitate, ad esempio un database per l'elaborazione di transazioni online (OLTP, Online Transaction Processing) in una macchina virtuale. Questi miglioramenti sono evidenti quando si usano interfacce di rete con velocità più elevata, ad esempio Ethernet a 40 Gbps e InfiniBand da 56 Gbps.  |
| Limiti della larghezza di banda SMB | Nuova | È ora possibile usare [set-SmbBandwidthLimit](https://docs.microsoft.com/powershell/module/smbshare/set-smbbandwidthlimit) per impostare i limiti della larghezza di banda in tre categorie: virtualmachine (Hyper-v su traffico SMB), LiveMigration (traffico Live Migration Hyper-v su SMB) o default (tutti gli altri tipi di traffico SMB).

Per ulteriori informazioni sulle funzionalità SMB nuove e modificate in Windows Server 2012 R2, vedere Novità di [SMB in Windows Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>).

## <a name="features-added-in-smb-30-with-windows-server-2012-and-windows-8"></a>Funzionalità aggiunte in SMB 3,0 con Windows Server 2012 e Windows 8

| Caratteristica/funzionalità  | Novità o aggiornamento  | Riepilogo  |
| --------- | --------- | --------- |
| Failover trasparente SMB     |   Nuova    | Consente agli amministratori di eseguire la manutenzione hardware o software di nodi in un file server in cluster senza interrompere la memorizzazione di dati su tali condivisioni file da parte delle applicazioni server. Inoltre, nell'eventualità di un errore hardware o software in un nodo del cluster, i SMB client si riconnettono in modo trasparente a un altro nodo del cluster senza interrompere le applicazioni server che memorizzano i dati in tali condivisioni file.        |
| Scalabilità orizzontale SMB     |   Nuova      | Supporto per più istanze SMB in un File server di scalabilità orizzontale. Usando Volumi condivisi cluster versione 2, gli amministratori possono creare condivisioni file che forniscono l'accesso simultaneo a file di dati, con I/O diretto, attraverso tutti i nodi in un cluster di file server. Questo offre un utilizzo migliore della larghezza di banda di rete e del bilanciamento del carico dei client file server, e ottimizza le prestazioni per applicazioni server.  |
| SMB multicanale     |   Nuova      |  Consente l'aggregazione della larghezza di banda di rete e della tolleranza di errore di rete se sono disponibili più percorsi tra il client e il server SMB. In questo modo le applicazioni server possono sfruttare appieno tutta la larghezza di banda disponibile ed essere resilienti agli errori della rete.<br><br>SMB multicanale in SMB 3 contribuisce a un miglioramento sostanziale delle prestazioni rispetto alle versioni precedenti di SMB. |
| SMB diretto     |   Nuova      | Supporta l'uso di schede di rete con capacità RDMA e può funzionare alla massima velocità con una latenza molto bassa e un uso minimo della CPU. Nei carichi di lavoro come Hyper-V o Microsoft SQL Server, ciò consente di assimilare un file server remoto a un archivio locale.<br><br>SMB diretto in SMB 3 contribuisce a un aumento sostanziale delle prestazioni rispetto alle versioni precedenti di SMB.  |
| Contatori delle prestazioni per applicazioni server     |   Nuova      |  I nuovi contatori delle prestazioni SMB offrono informazioni dettagliate, per condivisione sulla velocità effettiva, la latenza e I/O al secondo (IOPS), consentendo agli amministratori di analizzare le prestazioni delle condivisioni file SMB in cui sono archiviati i dati. Questi contatori sono appositamente progettati per le applicazioni server, ad esempio Hyper-V e SQL Server, che archiviano file in condivisioni file remote.      |
| Ottimizzazioni delle prestazioni    |  Aggiornamento   | Il client e il server SMB sono stati ottimizzati per i/O di lettura/scrittura casuali di piccole dimensioni, che sono comuni nelle applicazioni server come SQL Server OLTP. Inoltre, per impostazione predefinita è attivata una grande unità massima di trasmissione (MTU, maximum transmission unit), che migliora notevolmente le prestazioni nei trasferimenti sequenziali di grandi dimensioni, ad esempio data warehouse di SQL Server, backup di database o ripristino, distribuzione o copia di dischi rigidi virtuali. |
| Cmdlet di Windows PowerShell specifici per SMB     |   Nuova      |  Con i cmdlet di Windows PowerShell per SMB gli amministratori possono gestire condivisioni file nel file server, end-to-end, dalla riga di comando.   |
| Crittografia SMB     |   Nuova      | Fornisce la crittografia end-to-end dei dati SMB e protegge i dati dalle occorrenze di intercettazione su reti non attendibili. Non comporta nuovi costi di distribuzione e non richiede Internet Protocol Security (IPsec), hardware specializzato o acceleratori della WAN. Può essere configurata in base alle singole condivisioni o per l'intero file server e può essere abilitata per un'ampia gamma di scenari in cui i dati attraversano reti non attendibili. |
| Leasing di directory SMB     |  Nuova | Migliora i tempi di risposta delle applicazioni nelle succursali. L'uso di lease di directory riduce i roundtrip dal client al server perché i metadati vengono recuperati da una cache di directory di maggiore durata. La coerenza della cache viene mantenuta perché i client ricevono una notifica quando vengono modificate le informazioni di directory nel server. I lease di directory funzionano con scenari per HomeDirectory (lettura/scrittura senza condivisione) e pubblicazione (sola lettura con condivisione).    |
| Prestazioni sulla rete WAN   | Nuova   | I lease di oplock (directory opportunistical Locks) e blocco opportunistico sono stati introdotti in SMB 3,0. Per i carichi di lavoro di Office/client tipici, vengono visualizzati oplock/lease per ridurre i round trip di rete di circa il 15%.<br><br>In SMB 3, l'implementazione di Windows di SMB è stata perfezionata per migliorare il comportamento di memorizzazione nella cache nel client, oltre alla possibilità di effettuare il push di velocità effettiva più elevate.<br><br>SMB 3 migliora le funzionalità dell'API CopyFile (), nonché gli strumenti associati, ad esempio Robocopy, per eseguire il push di più dati in rete. |
| Negoziazione sicura del dialetto | Nuova | Aiuta a proteggersi dal tentativo Man-in-the-Middle di effettuare il downgrade della negoziazione del dialetto. L'idea è impedire a un utente malintenzionato di effettuare il downgrade del dialetto e delle funzionalità inizialmente negoziato tra il client e il server. Per informazioni dettagliate, vedere [negoziazione del dialetto sicuro SMB3](https://docs.microsoft.com/archive/blogs/openspecification/smb3-secure-dialect-negotiation). Si noti che questa operazione è stata sostituita dalla funzionalità di [integrità pre-autenticazione SMB 3.1.1 in Windows 10](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10) in SMB 3.1.1. |


## <a name="hardware-requirements"></a>Requisiti hardware

Il failover trasparente di SMB ha i seguenti requisiti:

* Un cluster di failover che esegue Windows Server 2012 o Windows Server 2016 con almeno due nodi configurati. Il cluster deve superare i test di convalida inclusi nella procedura guidata di convalida.
* Le condivisioni file devono essere create con la proprietà disponibilità continua (CA), che è l'impostazione predefinita.
* Le condivisioni file devono essere create in percorsi dei volumi condivisi cluster per ottenere la scalabilità orizzontale SMB.
* Nei computer client deve essere in esecuzione Windows® 8 o Windows Server 2012, entrambi i quali includono il client SMB aggiornato che supporta la disponibilità continua.

> [!NOTE]
> I client di livello inferiore possono connettersi alle condivisioni file che hanno la proprietà CA, ma il failover trasparente non sarà supportato per questi client.

I requisiti di SMB multicanale sono i seguenti:

* Sono necessari almeno due computer che eseguono Windows Server 2012. Non è necessario installare alcuna funzionalità aggiuntiva: la tecnologia è attivata per impostazione predefinita.
* Per informazioni sulle configurazioni di rete consigliate, vedere la sezione Vedere anche alla fine di questo argomento introduttivo.

I requisiti di SMB diretto sono i seguenti:

* Sono necessari almeno due computer che eseguono Windows Server 2012. Non è necessario installare alcuna funzionalità aggiuntiva: la tecnologia è attivata per impostazione predefinita.
* Sono necessarie una o più schede di rete con funzionalità RDMA. Tali schede sono attualmente disponibili in tre tipologie: iWARP, Infiniband o RoCE (RDMA over Converged Ethernet).

## <a name="more-information"></a>Altre informazioni

Nell'elenco seguente sono disponibili risorse aggiuntive sul Web relative a SMB e alle tecnologie correlate in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.

* [Archiviazione in Windows Server](../storage.md)
* [File server di scalabilità orizzontale per i dati delle applicazioni](../../failover-clustering/sofs-overview.md)
* [Migliorare le prestazioni di un file server con SMB diretto](smb-direct.md)
* [Distribuire Hyper-V tramite SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [Distribuire SMB multicanale](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [Distribuzione di file server veloci ed efficienti per applicazioni server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB: Guida alla risoluzione dei problemi](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)
