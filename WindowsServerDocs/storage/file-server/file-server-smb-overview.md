---
title: Panoramica della condivisione file usando il protocollo SMB 3 in Windows Server
description: Panoramica dell'uso il protocollo SMB 3 per condivisioni file e file server con Windows Server.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: fc4c8b341ee78db80f862ee412400f0a930fe810
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845052"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>Panoramica della condivisione file usando il protocollo SMB 3 in Windows Server

>Si applica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

In questo argomento descrive la funzionalità SMB 3.0 in Windows Server® 2012, Windows Server 2012 R2 e Windows Server 2016, ovvero utilizzi pratici per la funzionalità, più significativa nuove o aggiornate funzionalità in questa versione rispetto alle versioni precedenti e l'hardware requisiti.

## <a name="feature-description"></a>Descrizione delle caratteristiche

SMB (Server Message Block) è un protocollo di condivisione di file di rete che consente alle applicazioni in un computer di leggere e scrivere da/su file, nonché di richiedere servizi da programmi server in una rete di computer. Il protocollo SMB può essere usato in aggiunta al proprio protocollo TCP/IP o ad altri protocolli di rete. Usando il protocollo SMB, un'applicazione (o l'utente di un'applicazione) può accedere a file o ad altre risorse presso un server remoto. Ciò consente alle applicazioni di leggere, creare e aggiornare file nel server remoto. Può anche comunicare con qualsiasi programma server configurato per ricevere una richiesta client SMB. Windows Server 2012 introduce la nuova versione del protocollo SMB 3.0.

## <a name="practical-applications"></a>Applicazioni pratiche

Questa sezione illustra alcuni nuovi metodi pratici per usare il nuovo protocollo SMB 3.0.

* **Archiviazione di file per la virtualizzazione (Hyper-V ™ su SMB)**. Hyper-V può archiviare i file di una macchina virtuale, i file di configurazione, i file del disco rigido virtuale (VHD) e gli snapshot nelle condivisioni file SMB 3.0. Questa funzionalità può essere usata per file server autonomi e file server in cluster che usano Hyper-V con archiviazione di file condivisi per il cluster.
* **Microsoft SQL Server su SMB**. SQL Server può archiviare file del database utente nelle condivisioni file SMB. Attualmente questa funzionalità è supportata con SQL Server 2008 R2 per i server SQL autonomi. Nelle prossime versioni di SQL Server verrà aggiunto il supporto per SQL Server in cluster e database di sistema.
* **Archiviazione tradizionale per i dati dell'utente finale**. Il protocollo SMB 3.0 fornisce miglioramenti per i carichi di lavoro degli Information Worker. Questi miglioramenti includono la riduzione delle latenze di applicazione riscontrate dagli utenti delle succursali durante l'accesso ai dati su una rete WAN (Wide Area Network) e la protezione dei dati da attacchi di intercettazione.

## <a name="new-and-changed-functionality"></a>Funzionalità nuove e modificate

Per informazioni sulle funzionalità nuove e modificate in Windows Server 2012 R2, vedere [What ' s New in SMB in Windows Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>).

SMB in Windows Server 2012 e Windows Server 2016 include il nuovo protocollo SMB 3.0 e diversi nuovi miglioramenti descritti nella tabella seguente.

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
<td><p>Nuova</p></td>
<td><p>Consente agli amministratori di eseguire la manutenzione hardware o software di nodi in un file server in cluster senza interrompere la memorizzazione di dati su tali condivisioni file da parte delle applicazioni server. Inoltre, nell'eventualità di un errore hardware o software in un nodo del cluster, i SMB client si riconnettono in modo trasparente a un altro nodo del cluster senza interrompere le applicazioni server che memorizzano i dati in tali condivisioni file.</p></td>
</tr>
<tr class="even">
<td><p>Scalabilità orizzontale SMB</p></td>
<td><p>Nuova</p></td>
<td><p>Usando Volumi condivisi cluster versione 2, gli amministratori possono creare condivisioni file che forniscono l'accesso simultaneo a file di dati, con I/O diretto, attraverso tutti i nodi in un cluster di file server. Questo offre un utilizzo migliore della larghezza di banda di rete e del bilanciamento del carico dei client file server, e ottimizza le prestazioni per applicazioni server.</p></td>
</tr>
<tr class="odd">
<td><p>SMB multicanale</p></td>
<td><p>Nuova</p></td>
<td><p>Consente l'aggregazione della larghezza di banda e della tolleranza di errore della rete se sono disponibili più percorsi tra il client SMB 3.0 e il server SMB 3.0. In questo modo le applicazioni server possono sfruttare appieno tutta la larghezza di banda disponibile ed essere resilienti agli errori della rete.</p></td>
</tr>
<tr class="even">
<td><p>SMB diretto</p></td>
<td><p>Nuova</p></td>
<td><p>Supporta l'uso di schede di rete con capacità RDMA e può funzionare alla massima velocità con una latenza molto bassa e un uso minimo della CPU. Nei carichi di lavoro come Hyper-V o Microsoft SQL Server, ciò consente di assimilare un file server remoto a un archivio locale.</p></td>
</tr>
<tr class="odd">
<td><p>Contatori delle prestazioni per applicazioni server</p></td>
<td><p>Nuova</p></td>
<td><p>I nuovi contatori delle prestazioni SMB offrono informazioni dettagliate, per condivisione, relative a velocità effettiva, latenza e I/O al secondo (IOPS), consentendo agli amministratori di analizzare le prestazioni delle condivisioni file di SMB 3.0 in cui sono archiviati i dati. Questi contatori sono appositamente progettati per le applicazioni server, ad esempio Hyper-V e SQL Server, che archiviano file in condivisioni file remote.</p></td>
</tr>
<tr class="even">
<td><p>Ottimizzazioni delle prestazioni</p></td>
<td><p>Aggiornamento</p></td>
<td><p>Sia il client SMB 3.0 che il server SMB 3.0 sono stati ottimizzati per gli I/O di lettura/scrittura casuali di piccole dimensioni, che sono comuni nelle applicazioni server come OLTP di SQL Server. Inoltre, per impostazione predefinita è attivata una grande unità massima di trasmissione (MTU, maximum transmission unit), che migliora notevolmente le prestazioni nei trasferimenti sequenziali di grandi dimensioni, ad esempio data warehouse di SQL Server, backup di database o ripristino, distribuzione o copia di dischi rigidi virtuali.</p></td>
</tr>
<tr class="odd">
<td><p>Cmdlet di Windows PowerShell specifici per SMB</p></td>
<td><p>Nuova</p></td>
<td><p>Con i cmdlet di Windows PowerShell per SMB gli amministratori possono gestire condivisioni file nel file server, end-to-end, dalla riga di comando.</p></td>
</tr>
<tr class="even">
<td><p>Crittografia SMB</p></td>
<td><p>Nuova</p></td>
<td><p>Fornisce la crittografia end-to-end dei dati SMB e protegge i dati dalle occorrenze di intercettazione su reti non attendibili. Non comporta nuovi costi di distribuzione e non richiede Internet Protocol Security (IPsec), hardware specializzato o acceleratori della WAN. Può essere configurata in base alle singole condivisioni o per l'intero file server e può essere abilitata per un'ampia gamma di scenari in cui i dati attraversano reti non attendibili.</p></td>
</tr>
<tr class="odd">
<td><p>Leasing di directory SMB</p></td>
<td><p>Nuova</p></td>
<td><p>Migliora i tempi di risposta delle applicazioni nelle succursali. L'uso di lease di directory riduce i roundtrip dal client al server perché i metadati vengono recuperati da una cache di directory di maggiore durata. La coerenza della cache viene mantenuta perché i client ricevono una notifica quando vengono modificate le informazioni di directory nel server. Funziona con scenari di <em>HomeDirectory</em> (lettura/scrittura senza condivisione) e <em>pubblicazione</em> (sola lettura con la condivisione).</p></td>
</tr>
</tbody>
</table>

## <a name="hardware-requirements"></a>Requisiti hardware

Il failover trasparente di SMB ha i seguenti requisiti:

* Un cluster di failover che esegue Windows Server 2012 o Windows Server 2016 con almeno due nodi configurati. Il cluster deve superare i test di convalida inclusi nella procedura guidata di convalida.
* Le condivisioni file devono essere create con la proprietà disponibilità continua (CA), che è l'impostazione predefinita.
* Le condivisioni file devono essere create in percorsi dei volumi condivisi cluster per ottenere la scalabilità orizzontale SMB.
* I computer client devono essere in esecuzione Windows® 8 o Windows Server 2012, che includono entrambe il client SMB aggiornato che supporta la disponibilità continua.

>[!NOTE]
>I client di livello inferiore possono connettersi alle condivisioni file che hanno la proprietà CA, ma non sarà supportato il failover trasparente per questi client.

I requisiti di SMB multicanale sono i seguenti:

* Sono necessari almeno due computer che eseguono Windows Server 2012. Non è necessario installare alcuna funzionalità aggiuntiva: la tecnologia è attivata per impostazione predefinita.
* Per informazioni sulle configurazioni di rete consigliate, vedere la sezione Vedere anche alla fine di questo argomento introduttivo.

I requisiti di SMB diretto sono i seguenti:

* Sono necessari almeno due computer che eseguono Windows Server 2012. Non è necessario installare alcuna funzionalità aggiuntiva: la tecnologia è attivata per impostazione predefinita.
* Sono necessarie una o più schede di rete con funzionalità RDMA. Tali schede sono attualmente disponibili in tre tipologie: iWARP, Infiniband o RoCE (RDMA over Converged Ethernet).

## <a name="more-information"></a>Altre informazioni

Nell'elenco seguente vengono fornite risorse aggiuntive sul web relative a SMB e alle tecnologie correlate in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.

* [Archiviazione in Windows Server](../storage.md)
* [Scale-Out File Server per i dati dell'applicazione](../../failover-clustering/sofs-overview.md)
* [Migliorare le prestazioni di un File Server con SMB diretto](smb-direct.md)
* [Distribuire Hyper-V tramite SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [Deploy SMB Multichannel](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [Distribuzione di File server veloci ed efficienti per applicazioni Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB: Guida alla risoluzione dei problemi](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)