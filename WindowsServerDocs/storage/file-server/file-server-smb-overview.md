---
title: Panoramica della condivisione file utilizzando il protocollo SMB 3 in Windows Server
description: Panoramica dell'utilizzo del protocollo SMB 3 per l'esecuzione di file con Windows Server e condivisioni di file.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: fc4c8b341ee78db80f862ee412400f0a930fe810
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233487"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>Panoramica della condivisione file utilizzando il protocollo SMB 3 in Windows Server

>Si applica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

In questo argomento viene descritta la funzionalità 3.0 SMB in Windows Server® 2012, Windows Server 2012 R2 e Windows Server 2016, ovvero pratico viene utilizzato per la funzionalità più importante nuove o aggiornate funzionalità in questa versione rispetto alle versioni precedenti e l'hardware requisiti.

## <a name="feature-description"></a>Descrizione delle caratteristiche

SMB (Server Message Block) è un protocollo di condivisione di file di rete che consente alle applicazioni in un computer di leggere e scrivere da/su file, nonché di richiedere servizi da programmi server in una rete di computer. Il protocollo SMB può essere usato in aggiunta al proprio protocollo TCP/IP o ad altri protocolli di rete. Usando il protocollo SMB, un'applicazione (o l'utente di un'applicazione) può accedere a file o ad altre risorse presso un server remoto. Ciò consente alle applicazioni di leggere, creare e aggiornare file nel server remoto. Può anche comunicare con qualsiasi programma server configurato per ricevere una richiesta client SMB. Windows Server 2012 introduce la nuova versione 3.0 del protocollo SMB.

## <a name="practical-applications"></a>Applicazioni pratiche

In questa sezione vengono illustrati alcuni metodi pratici nuovo tramite il protocollo SMB 3.0 nuovo.

* **Archiviazione dei file per la virtualizzazione (Hyper-V™ su SMB)**. Hyper-V è possibile archiviare file macchina virtuale, ad esempio di configurazione, file (VHD) nel disco rigido virtuale e gli snapshot, in condivisioni di file tramite il protocollo SMB 3.0. Può essere utilizzato per file server autonomi e non in pila file server che utilizzano la tecnologia Hyper-V con la memorizzazione dei file condiviso per il cluster.
* **Microsoft SQL Server su SMB**. SQL Server può archiviare i file di database utente SMB condivisioni di file. Attualmente, questo è supportato con SQL Server 2008 R2 per i server SQL autonomo. Le versioni future di SQL Server verranno aggiunto il supporto per i cluster di SQL Server e database di sistema.
* **Archiviazione tradizionali per i dati dell'utente finale**. Il protocollo SMB 3.0 offre miglioramenti per Information Worker (o client) carichi di lavoro. Questi miglioramenti includono riducendo le latenze applicazione che si sono verificate da utenti delle succursali quando gli attacchi di accesso ai dati sulle reti WAN (WAN) e la protezione dei dati da intercettazioni.

## <a name="new-and-changed-functionality"></a>Funzionalità nuove e modificate

Per informazioni sulle funzionalità nuove e modificate in Windows Server 2012 R2, vedere [What's New in SMB in Windows Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>).

SMB in Windows Server 2012 e Windows Server 2016 include il nuovo protocollo SMB 3.0 e numerosi miglioramenti nuovi descritte nella tabella seguente.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Caratteristica/funzionalità</p></th>
<th><p>Novità o aggiornamento</p></th>
<th><p>Riepilogo</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Failover trasparente SMB</p></td>
<td><p>Nuovo</p></td>
<td><p>Consente agli amministratori di eseguire la manutenzione di hardware o software di nodi in un file server cluster senza interrompere le applicazioni server archiviazione dei dati in tali condivisioni di file. Inoltre, se si verifica un errore hardware o software in un nodo cluster, i client SMB in modo trasparente riconnettono a un altro nodo del cluster senza interrompere le applicazioni server che memorizzano i dati in tali condivisioni di file.</p></td>
</tr>
<tr class="even">
<td><p>Scalabilità SMB</p></td>
<td><p>Nuovo</p></td>
<td><p>Utilizzo di Cluster condiviso volumi (con estensione CSV) versione 2, gli amministratori possono creare condivisioni file che forniscono accesso simultaneo ai file di dati con i/o diretto, a tutti i nodi del cluster file server. Vengono fornite migliore utilizzo della larghezza di banda e bilanciamento del carico dei client file server e consente di ottimizzare le prestazioni per le applicazioni server.</p></td>
</tr>
<tr class="odd">
<td><p>Multicanale SMB</p></td>
<td><p>Nuovo</p></td>
<td><p>Consente l'aggregazione di larghezza di banda e la tolleranza di rete se più percorsi sono disponibili tra il client SMB 3.0 e il server SMB 3.0. In questo modo le applicazioni server per sfruttare appieno tutti disponibile larghezza di banda ed essere resiliente a un errore di rete.</p></td>
</tr>
<tr class="even">
<td><p>Diretto SMB</p></td>
<td><p>Nuovo</p></td>
<td><p>Supporta l'utilizzo di schede di rete che presentano funzionalità RDMA e può funzionare alla massima velocità con molto bassa latenza, durante l'utilizzo della CPU poche. Per carichi di lavoro, ad esempio Hyper-V o Microsoft SQL Server, in questo modo un file server remoto simile a quella di archiviazione locale.</p></td>
</tr>
<tr class="odd">
<td><p>Contatori delle prestazioni per le applicazioni server</p></td>
<td><p>Nuovo</p></td>
<td><p>Il nuovo prestazioni SMB forniscono inoltre dettagliati, per-Condividi informazioni sulla velocità effettiva, latenza e i/o al secondo (IOPS), che consente agli amministratori di analizzare le prestazioni delle condivisioni di file SMB 3.0 cui sono archiviati i dati. Questi contatori sono progettati per le applicazioni server, ad esempio Hyper-V e SQL Server in cui archiviare i file in condivisioni di file remoto.</p></td>
</tr>
<tr class="even">
<td><p>Ottimizzazioni delle prestazioni</p></td>
<td><p>Aggiornato</p></td>
<td><p>Il client SMB 3.0 e server SMB 3.0 sono state ottimizzate per i/o di piccole dimensioni casuali di lettura/scrittura, ovvero comune in server applicazioni, ad esempio OLTP di SQL Server. Inoltre, grande MTU Maximum Transmission Unit () è attivata per impostazione predefinita, che migliora notevolmente le prestazioni in sequenziale trasferimenti di grandi dimensioni, ad esempio data warehouse SQL Server, database backup o ripristino, distribuzione o copia di dischi rigidi virtuali.</p></td>
</tr>
<tr class="odd">
<td><p>Cmdlet di Windows PowerShell SMB specifici</p></td>
<td><p>Nuovo</p></td>
<td><p>I cmdlet di Windows PowerShell per SMB, gli amministratori possono gestire condivisioni di file nel file server to end, dalla riga di comando.</p></td>
</tr>
<tr class="even">
<td><p>Crittografia SMB</p></td>
<td><p>Nuovo</p></td>
<td><p>Fornisce la crittografia dei dati SMB end-to-end e protegge i dati da eavesdropping occorrenze su reti non attendibile. È necessario alcun nuovi costi di distribuzione e non è necessario Internet Protocol security (IPsec), hardware specializzata o acceleratori WAN. È possibile configurare per azione, o per l'intero file server e può essere abilitato per una serie di scenari in cui dati attraversa reti non attendibili.</p></td>
</tr>
<tr class="odd">
<td><p>Leasing Directory SMB</p></td>
<td><p>Nuovo</p></td>
<td><p>Consente di migliorare i tempi di risposta dell'applicazione nelle succursali. Con l'utilizzo di lease di directory, round trip dal client al server viene ridotto dopo il recupero dei metadati da più tempo professione della cache della directory. Coerenza della cache viene gestita dal momento che i client vengono informati quando vengono modificate le informazioni di directory nel server. Funziona con gli scenari di <em>HomeFolder</em> (lettura/scrittura alcuna condivisione) e la <em>pubblicazione</em> (sola lettura con la condivisione).</p></td>
</tr>
</tbody>
</table>

## <a name="hardware-requirements"></a>Requisiti hardware

Failover trasparente SMB presenta i requisiti seguenti:

* Un cluster di failover che esegue Windows Server 2012 o Windows Server 2016 con almeno due nodi configurati. Il cluster deve superare i test di convalida dei cluster inclusi nella convalida guidata.
* Condivisioni di file devono essere create con la proprietà continua disponibilità (CA), per impostazione predefinita.
* Condivisioni di file devono essere create in percorsi del volume CSV in modo da riflettere SMB con scalabilità orizzontale.
* Computer client deve essere in esecuzione Windows® 8 o Windows Server 2012, che includono il client SMB aggiornato che supporta la disponibilità continua.

>[!NOTE]
>Livello inferiore i client possano connettersi a condivisioni file in cui la proprietà CA, ma non sarà supportato failover trasparente per questi client.

Multicanale SMB presenta i requisiti seguenti:

* Sono necessari almeno due computer che eseguono Windows Server 2012. Nessuna funzionalità aggiuntiva desidera essere installati, ovvero la tecnologia è attivata per impostazione predefinita.
* Per informazioni sulle configurazioni della rete consigliati, vedere la sezione Vedere anche alla fine di questo argomento di panoramica.

Diretto SMB presenta i requisiti seguenti:

* Sono necessari almeno due computer che eseguono Windows Server 2012. Nessuna funzionalità aggiuntiva desidera essere installati, ovvero la tecnologia è attivata per impostazione predefinita.
* Schede di rete con funzionalità RDMA sono necessari. Attualmente, queste schede sono disponibili tre diversi tipi: iWARP Infiniband o investito (RDMA over Ethernet convergenza).

## <a name="more-information"></a>Ulteriori informazioni

Nell'elenco seguente vengono fornite risorse aggiuntive sul web relative alla SMB e le tecnologie correlate in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.

* [Archiviazione in WindowsServer](../storage.md)
* [File di scalabilità orizzontale Server dei dati delle applicazioni](../../failover-clustering/sofs-overview.md)
* [Migliorare le prestazioni di un File Server con SMB diretta](smb-direct.md)
* [Distribuire Hyper-V tramite SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [Distribuire multicanale SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [Distribuzione di server File veloce ed efficiente per le applicazioni Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB: Guida alla risoluzione dei problemi](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)