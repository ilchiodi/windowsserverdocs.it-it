---
title: Archiviazione
description: ''
author: JasonGerend
manager: elizapo
layout: LandingPage
ms.prod: windows-server-threshold
ms.technology: storage
ms.assetid: 6b74bc7c-a58d-4915-af8e-2cc27f2c4726
ms.topic: landing-page
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 03/08/2019
ms.openlocfilehash: d83d51ebf56d38f93c176d403ea5c6c14f625ee2
ms.sourcegitcommit: 419bf9d6d18e7a32cba2cbcd98587b81a79adc51
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2019
ms.locfileid: "9152178"
---
# Archiviazione

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (Canale semestrale)

>[!TIP]
> Cerchi informazioni sull'installazione delle versioni precedenti di Windows Server? Vedi le nostre altre [librerie di Windows Server](/previous-versions/windows/) in docs.microsoft.com. Puoi anche [cercare nel sito](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) informazioni specifiche.

<hr />
L'archiviazione in Windows Server offre funzionalità nuove e migliorate per i clienti di data center software-defined che usano principalmente carichi di lavoro virtualizzati. WindowsServer offre inoltre un supporto completo per i clienti aziendali che usano file server con carichi di lavoro esistenti.

<hr />
<ul class="cardsF panelContent">
<li>
 <a href="whats-new-in-storage.md">
                            <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-whats-new.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                            <h2>Novità</h2>
                                            <p>Scoprire le novità in Windows Server Storage</p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
</ul>
<hr />
<ul class="cardsF panelContent">
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Archiviazione software-defined per carichi di lavoro virtualizzati</h3>
<HR />
                        <p><h3><a href="storage-spaces/storage-spaces-direct-overview.md">Spazi di archiviazione diretta</a></h3> Associate direttamente l'archiviazione locale, inclusi i dispositivi SATA e NVME, per ottimizzare l'utilizzo del disco dopo l'aggiunta di nuovi dischi fisici e ripristinare volte per più veloce disco virtuale. Per informazioni sugli spazi di archiviazione autonomi e SAS, vedi anche <a href="storage-spaces/overview.md">Spazi di archiviazione</a> .</p>
<HR />
                        <p><h3><a href="storage-replica/storage-replica-overview.md">Replica di archiviazione</a></h3> Replica archiviazione indipendente, a livello di blocco e sincrona tra cluster o server per la preparazione di emergenza e ripristino, nonché estensione di un cluster di failover tra siti per una disponibilità elevata. La replica sincrona consente il mirroring dei dati in siti fisici con volumi coerenti per arresto anomalo del sistema senza perdere dati a livello di file system.</p>
<HR />
                        <p><h3><a href="storage-qos/storage-qos-overview.md">QoS di archiviazione</a></h3> Monitorare e gestire le prestazioni di archiviazione delle macchine virtuali usando Hyper-V e i ruoli del Server di scalabilità orizzontale File automaticamente centralmente improveing equità delle risorse di archiviazione tra più macchine virtuali usando lo stesso cluster di file server.</p>
<HR />
                        <p><h3><a href="data-deduplication/overview.md">Deduplicazione dati</a></h3> Consente di ottimizzare lo spazio libero su un volume esaminandone i dati nel volume di duplicazione. Una volta individuate, le parti duplicate del set di dati del volume vengono archiviate una sola volta e possono essere compresse per risparmi aggiuntivi. Deduplicazione dati ottimizza le ridondanze senza compromettere la fedeltà o l'integrità dei dati.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>File server per utilizzo generico</h3>
<HR />
                        <p><h3><a href="storage-migration-service/overview.md">Servizio di migrazione della risorsa di archiviazione</a></h3>Eseguire la migrazione di server a una versione più recente di Windows Server con uno strumento grafico che inventario dei dati nei server, trasferisce i dati e la configurazione server più recenti e, facoltativamente, sposta le identità dei server precedenti ai nuovi server in modo che le App e gli utenti non dovrai apportare alcuna modifica.</p>
<HR />
                        <p><h3><a href="work-folders/work-folders-overview.md">Cartelle di lavoro</a></h3> Archiviare e accedere file su PC e dispositivi, spesso indicati come bring your own device (BYOD), oltre che sui PC aziendali di lavoro. Gli utenti avranno a disposizione una posizione in cui archiviare i file di lavoro a cui possono accedere da qualsiasi luogo. Le organizzazioni mantengono il controllo sui dati aziendali archiviando i file in file server gestiti centralmente e, facoltativamente, specificando criteri per i dispositivi degli utenti, ad esempio le password di crittografia e schermata di blocco.</p>
<HR />
                        <p><h3><a href="folder-redirection/folder-redirection-rup-overview.md">File offline e reindirizzamento cartelle</a></h3> Reindirizzare il percorso di cartelle locali (ad esempio la cartella documenti) a un percorso di rete, mentre la memorizzazione nella cache il contenuto in locale per garantire maggiore velocità e disponibilità.</p>
<HR />
                        <p><h3><a href="folder-redirection/deploy-roaming-user-profiles.md">Profili utente mobili</a></h3> Reindirizzare un profilo utente su un percorso di rete.</p>
<HR />
                        <p><h3><a href="dfs-namespaces/dfs-overview.md">Spazi dei nomi DFS</a></h3> Raggruppare cartelle condivise che si trovano in server diversi in uno o più spazi dei nomi strutturati logicamente. Ogni spazio dei nomi viene visualizzato agli utenti come un'unica cartella condivisa con una serie di sottocartelle. La struttura sottostante dello spazio dei nomi può tuttavia essere costituita da numerose condivisioni file situate in server diversi e in più siti.</p>
<HR />
                        <p><h3><a href="dfs-replication/dfsr-overview.md">Replica DFS</a></h3> Replicare le cartelle (incluse quelle a cui fa riferimento un percorso di spazio dei nomi DFS) in più server e siti. Replica DFS utilizza un algoritmo di compressione noto come RDC (Remote Differential Compression). La tecnologia RDC è in grado di rilevare le modifiche apportate ai dati di un file e consente a Replica DFS di replicare solo i blocchi di file modificati anziché l'intero file.</p>
<HR />
                        <p><h3><a href="fsrm/fsrm-overview.md">Gestione risorse file server</a></h3> Gestire e classificare i dati archiviati nei file server.<p>
<HR />
                        <p><h3><a href="iscsi/iscsi-target-server.md">Server di destinazione iSCSI</a></h3> Fornisce l'archiviazione a blocchi per altri server e applicazioni nella rete tramite lo standard iSCSI (Internet SCSI).</p>
<HR />
                       <p><h3><a href="iscsi/iscsi-boot-overview.md">Server di destinazione iSCSI</a></h3> È in grado di avviare centinaia di computer da un'immagine del sistema operativo singolo archiviato in una posizione centralizzata. Si ottengono così miglioramenti a livello di efficienza, gestibilità, disponibilità e sicurezza.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>File system, protocolli, ecc.</h3>
<HR />
                        <p><h3><a href="refs/refs-overview.md">ReFS</a></h3> Un file system resiliente che ottimizza la disponibilità dei dati, consente un ridimensionamento efficiente a set di dati molto grandi in vari carichi di lavoro e garantisce l'integrità dei dati mezzo della resilienza ai danneggiamenti (indipendentemente dagli errori software o hardware).<p>
<HR />
                        <p><h3><a href="file-server/file-server-smb-overview.md">Protocollo di Server Message Block (SMB)</a></h3> Una rete protocollo di condivisione file che consente alle applicazioni in un computer per leggere e scrivere su file, nonché di richiedere servizi da programmi server in una rete di computer. Il protocollo SMB può essere usato in aggiunta al proprio protocollo TCP/IP o ad altri protocolli di rete. Usando il protocollo SMB, un'applicazione (o l'utente di un'applicazione) può accedere a file o ad altre risorse presso un server remoto. Ciò consente alle applicazioni di leggere, creare e aggiornare file nel server remoto. Può anche comunicare con qualsiasi programma server configurato per ricevere una richiesta client SMB.<p>
<HR />
                        <p><h3><a href="storage-spaces/Storage-class-memory-health.md">Memoria della classe di archiviazione</a></h3> Offre prestazioni simili alla memoria del computer (velocità elevata), ma con la persistenza dei dati delle unità di archiviazione normali. Windows gestisce la memoria della classe di archiviazione in modo simile alle unità normali, solo con una velocità maggiore, ma esistono alcune differenze nella modalità in cui viene gestita l'integrità del dispositivo.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/cc766295(v=ws.10).aspx">Crittografia unità BitLocker</a></h3> Archivia i dati nei volumi in formato crittografato, anche se il computer sia manomesso o il sistema operativo non è in esecuzione. In questo modo è possibile tutelarsi da attacchi offline, ovvero da attacchi effettuati disabilitando o aggirando il sistema operativo installato oppure rimuovendo fisicamente il disco rigido per intervenire separatamente sui dati.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/dn466522(v=ws.11).aspx">NTFS</a></h3> Il file system primario delle versioni recenti di Windows e Windows Server, offre un set completo di funzionalità che includono descrittori di sicurezza, crittografia, quote disco e metadata complessi e può essere usato con volumi condivisi Cluster (CSV) per fornire continuamente volumi disponibili a cui è possibile accedere contemporaneamente da più nodi di un Cluster di Failover.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/jj592688(v=ws.11).aspx">Network File System (NFS)</a></h3> Offre una soluzione per le aziende con ambienti eterogenei costituiti da computer Windows e non Windows di condivisione file.<p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

---


## In Azure

* [Archiviazione di Azure](https://azure.microsoft.com/documentation/services/storage/)
* [Azure StorSimple](https://www.microsoft.com/en-us/cloud-platform/azure-storsimple)
