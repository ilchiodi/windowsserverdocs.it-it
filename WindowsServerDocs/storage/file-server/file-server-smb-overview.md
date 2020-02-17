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
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2020
ms.locfileid: "75919889"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>Panoramica della condivisione di file con il protocollo SMB 3 in Windows Server

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento descrive la funzionalità SMB 3 in Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012, gli usi pratici per la funzionalità, le più importanti funzionalità nuove o aggiornate di questa versione rispetto alle versioni precedenti e i requisiti hardware. SMB è anche un protocollo di infrastruttura usato da soluzioni [data center software-defined (SDDC)](../../sddc.md) quali ad esempio Spazi di archiviazione diretta, Replica di archiviazione e così via. Il protocollo SMB versione 3.0 è stato introdotto con Windows Server 2012 ed è stato migliorato in modo incrementale nelle versioni successive.

## <a name="feature-description"></a>Descrizione delle funzionalità

SMB (Server Message Block) è un protocollo di condivisione di file di rete che consente alle applicazioni in un computer di leggere e scrivere da/su file, nonché di richiedere servizi da programmi server in una rete di computer. Il protocollo SMB può essere usato in aggiunta al proprio protocollo TCP/IP o ad altri protocolli di rete. Usando il protocollo SMB, un'applicazione (o l'utente di un'applicazione) può accedere a file o ad altre risorse presso un server remoto. Ciò consente alle applicazioni di leggere, creare e aggiornare file nel server remoto. SMB può anche comunicare con qualsiasi programma server configurato per ricevere una richiesta client SMB. SMB è un protocollo di infrastruttura usato da tecnologie di elaborazione SDDC quali ad esempio Spazi di archiviazione diretta e Replica di archiviazione. Per altre informazioni, vedi [Data center software-defined di Windows Server](../../sddc.md).

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
| Capacità di richiedere write-through su disco in condivisioni file che non sono costantemente disponibili | Nuovo | Per fornire una garanzia aggiuntiva che le operazioni di scrittura su una condivisione file compiano il percorso completo nello stack software e hardware fino al disco fisico prima della restituzione del risultato come operazione di scrittura completata, puoi abilitare il write-through nella condivisione file usando il comando `NET USE /WRITETHROUGH` o il cmdlet `New-SMBMapping -UseWriteThrough` di PowerShell. L'uso del write-through comporta una riduzione delle prestazioni. Per altre informazioni, vedi il post di blog [Controlling write-through behaviors in SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-write-through-behaviors-in-smb/bc-p/1083417#M677) (Controllo dei comportamenti di write-through in SMB). |

## <a name="features-added-in-windows-server-version-1709-and-windows-10-version-1709"></a>Funzionalità aggiunte in Windows Server, versione 1709 e Windows 10, versione 1709

| Caratteristica/funzionalità  | Novità o aggiornamento  | Riepilogo  |
| --------- | --------- | --------- |
| L'accesso guest alle condivisioni file è disabilitato | Nuovo | Il client SMB non consente più le azioni seguenti: Accesso dell'account guest a un server remoto. Viene eseguito il fallback all'account guest dopo che sono state specificate credenziali non valide. Per informazioni dettagliate, vedi [Per impostazione predefinita, l'accesso guest in SMB2 è disabilitato in Windows](https://support.microsoft.com/help/4046019/guest-access-in-smb2-disabled-by-default-in-windows-10-and-windows-ser). | 
| Mapping globale SMB | Nuovo | Esegue il mapping di una condivisione SMB remota a una lettera di unità accessibile a tutti gli utenti nell'host locale, inclusi i contenitori. Questa operazione è necessaria per abilitare l'I/O del contenitore nel volume di dati per attraversare il punto di montaggio remoto. Tieni presente che quando usi il mapping globale SMB per i contenitori, tutti gli utenti nell'host contenitore possono accedere alla condivisione remota. Anche qualsiasi applicazione in esecuzione nell'host contenitore avrà l'accesso alla condivisione remota mappata. Per informazioni dettagliate, vedi il post di blog [Container Storage Support with Cluster Shared Volumes (CSV), Storage Spaces Direct, SMB Global Mapping](https://techcommunity.microsoft.com/t5/failover-clustering/container-storage-support-with-cluster-shared-volumes-csv/ba-p/372140) (Supporto di archiviazione contenitore con volumi condivisi cluster, spazi di archiviazione diretta (S2D), mapping globale SMB). |
| Controllo dialetti SMB | Nuovo | Puoi ora impostare i valori del Registro di sistema per controllare la versione SMB minima (dialetto) e la versione SMB massima usate. Per informazioni dettagliate, vedi il post di blog [Controlling SMB Dialects](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024) (Controllo dei dialetti SMB). |

## <a name="features-added-in-smb-311-with-windows-server-2016-and-windows-10-version-1607"></a>Funzionalità aggiunte in SMB 3.11 con Windows Server 2016 e Windows 10, versione 1607

| Caratteristica/funzionalità  | Novità o aggiornamento  | Riepilogo  |
| --------- | --------- | --------- |
| Crittografia SMB     |   Aggiornamento      | La crittografia SMB 3.1.1 con Advanced Encryption Standard-Galois/Counter Mode (AES-GCM) è più veloce della firma SMB o della crittografia SMB precedente con AES-CCM.   |
| Memorizzazione di directory nella cache | Nuovo | In SMB 3.1.1 sono stati apportati miglioramenti alla memorizzazione delle directory nella cache. I client Windows possono ora memorizzare nella cache directory molto più estese, contenenti circa 500.000 voci. I client Windows tenteranno di eseguire query sulle directory con buffer da 1 MB per ridurre i round trip e migliorare le prestazioni. |
| Integrità di preautenticazione | Nuovo |  In SMB 3.1.1 l'integrità di preautenticazione offre una maggiore protezione da attacchi man-in-the-middle di manomissione dei messaggi di autenticazione e creazione della connessione SMB. Per informazioni dettagliate, vedi [Integrità di preautenticazione SMB 3.1.1 in Windows 10](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10). |
| Miglioramenti della crittografia SMB | Nuovo | SMB 3.1.1 offre un meccanismo per negoziare l'algoritmo di crittografia in base alla connessione, con le opzioni per AES-128-CCM e AES-128-GCM. AES-128-GCM è l'impostazione predefinita per le nuove versioni di Windows, mentre le versioni precedenti continueranno a usare AES-128-CCM. |
| Supporto dell'aggiornamento in sequenza dei cluster | Nuovo | Abilita gli [aggiornamenti in sequenza dei cluster](../../failover-clustering/cluster-operating-system-rolling-upgrade.md) consentendo a SMB di supportare versioni massime diverse di SMB per i cluster in fase di aggiornamento. Per altri dettagli su come consentire la comunicazione SMB con versioni diverse (dialetti) del protocollo, vedi il post di blog [Controlling SMB Dialects](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024) (Controllo dei dialetti SMB). |
| Supporto client SMB diretto in Windows 10 | Nuovo | Windows 10 Enterprise, Windows 10 Education e Windows 10 Pro for Workstations ora includono il supporto client SMB diretto. |
| Supporto nativo per le chiamate API FileNormalizedNameInformation | Nuovo | Aggiunge il supporto nativo per l'esecuzione di query sul nome normalizzato di un file. Per informazioni dettagliate, vedi [FileNormalizedNameInformation](https://docs.microsoft.com/openspecs/windows_protocols/ms-fscc/20bcadba-808c-4880-b757-4af93e41edf6). |

Per altre informazioni, vedi il post di blog [Novità di SMB 3.1.1 in Windows Server 2016 Technical Preview 2](https://docs.microsoft.com/archive/blogs/josebda/whats-new-in-smb-3-1-1-in-the-windows-server-2016-technical-preview-2).

## <a name="features-added-in-smb-302-with-windows-server-2012-r2-and-windows-81"></a>Funzionalità aggiunte in SMB 3.02 con Windows Server 2012 R2 e Windows 8.1

| Caratteristica/funzionalità  | Novità o aggiornamento  | Riepilogo  |
| --------- | --------- | --------- |
| Ribilanciamento automatico dei client di file server di scalabilità orizzontale     |   Nuovo      | Migliora la scalabilità e la gestibilità per i file server di scalabilità orizzontale. Le connessioni client SMB vengono registrate per ogni condivisione file, anziché per ogni server, e i client vengono quindi reindirizzati al nodo del cluster con l'accesso migliore al volume utilizzato dalla condivisione file. In questo modo si ottiene un miglioramento dell'efficienza grazie alla riduzione del reindirizzamento del traffico tra i nodi del file server. I client vengono reindirizzati in seguito alla connessione iniziale e alla riconfigurazione dell'archiviazione del cluster.    |
| Prestazioni sulla rete WAN   | Aggiornamento  | Windows 8.1 e Windows 10 forniscono un supporto CopyFile SRV_COPYCHUNK su SMB migliorato quando viene usato Esplora file per le copie remote da un percorso su un computer remoto a un'altra copia nello stesso server. Copierai solo una piccola quantità di metadati sulla rete (vengono trasmessi 1/2KiB per 16MiB di dati di file). Questo comporta un miglioramento significativo delle prestazioni. Si tratta di una distinzione a livello di sistema operativo e di Esplora file per SMB. |
| SMB diretto     |   Aggiornamento      | Le prestazioni per i carichi di lavoro di I/O ridotti risultano migliorate grazie a una maggiore efficienza per l'hosting dei carichi di lavoro con operazioni di I/O limitate, ad esempio un database per l'elaborazione di transazioni online (OLTP, Online Transaction Processing) in una macchina virtuale. Questi miglioramenti sono evidenti quando vengono usate interfacce di rete ad alta velocità, come Ethernet da 40 Gbps e InfiniBand da 56 Gbps.  |
| Limite larghezza di banda SMB | Nuovo | Puoi usare ora [set-SmbBandwidthLimit](https://docs.microsoft.com/powershell/module/smbshare/set-smbbandwidthlimit) per impostare i limiti della larghezza di banda in tre categorie: VirtualMachine (traffico Hyper-V su SMB), LiveMigration (traffico Live Migration Hyper-V su SMB) o Default (tutti gli altri tipi di traffico SMB).

Per altre informazioni sulle funzionalità SMB nuove e modificate in Windows Server 2012 R2, vedi [Novità di SMB in Windows Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>).

## <a name="features-added-in-smb-30-with-windows-server-2012-and-windows-8"></a>Funzionalità aggiunte in SMB 3.0 con Windows Server 2012 e Windows 8

| Caratteristica/funzionalità  | Novità o aggiornamento  | Riepilogo  |
| --------- | --------- | --------- |
| Failover trasparente SMB     |   Nuovo    | Consente agli amministratori di eseguire la manutenzione hardware o software di nodi in un file server in cluster senza interrompere la memorizzazione di dati su tali condivisioni file da parte delle applicazioni server. Inoltre, nell'eventualità di un errore hardware o software in un nodo del cluster, i SMB client si riconnettono in modo trasparente a un altro nodo del cluster senza interrompere le applicazioni server che memorizzano i dati in tali condivisioni file.        |
| Scalabilità orizzontale SMB     |   Nuovo      | Supporto di più istanze di SMB in un file server di scalabilità orizzontale. Usando Volumi condivisi cluster versione 2, gli amministratori possono creare condivisioni file che forniscono l'accesso simultaneo a file di dati, con I/O diretto, attraverso tutti i nodi in un cluster di file server. Questo offre un utilizzo migliore della larghezza di banda di rete e del bilanciamento del carico dei client file server, e ottimizza le prestazioni per applicazioni server.  |
| SMB multicanale     |   Nuovo      |  Consente l'aggregazione della larghezza di banda e della tolleranza di errore della rete se sono disponibili più percorsi tra il server e il client SMB. In questo modo le applicazioni server possono sfruttare appieno tutta la larghezza di banda disponibile ed essere resilienti agli errori della rete.<br><br>SMB multicanale in SMB 3 contribuisce a un miglioramento sostanziale delle prestazioni rispetto alle versioni precedenti di SMB. |
| SMB diretto     |   Nuovo      | Supporta l'uso di schede di rete con capacità RDMA e può funzionare alla massima velocità con una latenza molto bassa e un uso minimo della CPU. Nei carichi di lavoro come Hyper-V o Microsoft SQL Server, ciò consente di assimilare un file server remoto a un archivio locale.<br><br>SMB diretto in SMB 3 contribuisce a un miglioramento sostanziale delle prestazioni rispetto alle versioni precedenti di SMB.  |
| Contatori delle prestazioni per applicazioni server     |   Nuovo      |  I nuovi contatori delle prestazioni SMB offrono informazioni dettagliate, per condivisione, relative a velocità effettiva, latenza e I/O al secondo (IOPS), consentendo agli amministratori di analizzare le prestazioni delle condivisioni file di SMB in cui sono archiviati i dati. Questi contatori sono appositamente progettati per le applicazioni server, ad esempio Hyper-V e SQL Server, che archiviano file in condivisioni file remote.      |
| Ottimizzazioni delle prestazioni    |  Aggiornamento   | Sia il server che il client SMB sono stati ottimizzati per gli I/O di lettura/scrittura casuali di piccole dimensioni, che sono comuni nelle applicazioni server come OLTP di SQL Server. Inoltre, per impostazione predefinita è attivata una grande unità massima di trasmissione (MTU, maximum transmission unit), che migliora notevolmente le prestazioni nei trasferimenti sequenziali di grandi dimensioni, ad esempio data warehouse di SQL Server, backup di database o ripristino, distribuzione o copia di dischi rigidi virtuali. |
| Cmdlet di Windows PowerShell specifici per SMB     |   Nuovo      |  Con i cmdlet di Windows PowerShell per SMB gli amministratori possono gestire condivisioni file nel file server, end-to-end, dalla riga di comando.   |
| Crittografia SMB     |   Nuovo      | Fornisce la crittografia end-to-end dei dati SMB e protegge i dati dalle occorrenze di intercettazione su reti non attendibili. Non comporta nuovi costi di distribuzione e non richiede Internet Protocol Security (IPsec), hardware specializzato o acceleratori della WAN. Può essere configurata in base alle singole condivisioni o per l'intero file server e può essere abilitata per un'ampia gamma di scenari in cui i dati attraversano reti non attendibili. |
| Leasing di directory SMB     |  Nuovo | Migliora i tempi di risposta delle applicazioni nelle succursali. L'uso di lease di directory riduce i roundtrip dal client al server perché i metadati vengono recuperati da una cache di directory di maggiore durata. La coerenza della cache viene mantenuta perché i client ricevono una notifica quando vengono modificate le informazioni di directory nel server. I lease di directory funzionano con scenari di HomeFolder (lettura/scrittura senza condivisione) e pubblicazione (sola lettura con la condivisione).    |
| Prestazioni sulla rete WAN   | Nuovo   | I blocchi opportunistici di directory (oplock) e i lease di oplock sono stati introdotti in SMB 3.0. Per carichi di lavoro di client/Office tipici, vengono visualizzati lease/oplock per ridurre i round trip di rete di circa il 15%.<br><br>In SMB 3 l'implementazione di SMB in Windows è stata perfezionata per migliorare il comportamento di memorizzazione nella cache nel client, oltre alla possibilità di velocità effettive più elevate.<br><br>In SMB 3 sono stati apportati miglioramenti all'API CopyFile(), nonché agli strumenti associati, ad esempio Robocopy, per eseguire il push di più dati sulla rete. |
| Negoziazione sicura dei dialetti | Nuovo | Offre protezione da tentativi di attacchi man-in-the-middle di downgrade della negoziazione dei dialetti. L'idea è impedire a un utente malintenzionato di effettuare il downgrade del dialetto inizialmente negoziato e delle funzionalità tra il client e il server. Per informazioni dettagliate, vedi [Negoziazione sicura dei dialetti SMB3](https://docs.microsoft.com/archive/blogs/openspecification/smb3-secure-dialect-negotiation). Tieni presente che questa operazione è stata sostituita dalla funzionalità [Integrità di preautenticazione in Windows 10](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10) in SMB 3.1.1. |


## <a name="hardware-requirements"></a>Requisiti hardware

Il failover trasparente di SMB ha i seguenti requisiti:

* Un cluster di failover che esegue Windows Server 2012 o Windows Server 2016 con almeno due nodi configurati. Il cluster deve superare i test di convalida inclusi nella procedura guidata di convalida.
* Le condivisioni file devono essere create con la proprietà disponibilità continua (CA), che è l'impostazione predefinita.
* Le condivisioni file devono essere create in percorsi dei volumi condivisi cluster per ottenere la scalabilità orizzontale SMB.
* I computer client devono eseguire Windows® 8 o Windows Server 2012, entrambi i quali includono il client SMB aggiornato che supporta la disponibilità continua.

> [!NOTE]
> I client di livello inferiore possono connettersi alle condivisioni file che hanno la proprietà CA, ma non sarà supportato il failover trasparente per questi client.

I requisiti di SMB multicanale sono i seguenti:

* Almeno due computer che eseguono Windows Server 2012. Non è necessario installare alcuna funzionalità aggiuntiva: la tecnologia è attivata per impostazione predefinita.
* Per informazioni sulle configurazioni di rete consigliate, vedere la sezione Vedere anche alla fine di questo argomento introduttivo.

I requisiti di SMB diretto sono i seguenti:

* Almeno due computer che eseguono Windows Server 2012. Non è necessario installare alcuna funzionalità aggiuntiva: la tecnologia è attivata per impostazione predefinita.
* Sono necessarie una o più schede di rete con funzionalità RDMA. Tali schede sono attualmente disponibili in tre tipologie: iWARP, Infiniband o RoCE (RDMA over Converged Ethernet).

## <a name="more-information"></a>Ulteriori informazioni

Nell'elenco seguente sono disponibili altre risorse di informazioni sul Web relative a SMB e alle tecnologie correlate in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.

* [Archiviazione in Windows Server](../storage.md)
* [File server di scalabilità orizzontale per dati delle applicazioni](../../failover-clustering/sofs-overview.md)
* [Migliorare le prestazioni di un file server con SMB diretto](smb-direct.md)
* [Distribuire Hyper-V tramite SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [Distribuire SMB multicanale](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [Distribuzione di file server veloci ed efficienti per applicazioni server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB: Guida alla risoluzione dei problemi](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)
