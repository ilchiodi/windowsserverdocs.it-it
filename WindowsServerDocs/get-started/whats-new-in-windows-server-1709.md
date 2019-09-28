---
title: Novità di Windows Server, versione 1709
description: Quali sono le nuove funzionalità di calcolo, identità, gestione, automazione, rete, sicurezza e archiviazione.
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: medium
ms.date: 06/03/2019
ms.openlocfilehash: 5dbbdc19707f2eadfa3b2c919af95b58645de441
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391370"
---
# <a name="whats-new-in-windows-server-version-1709"></a>Novità di Windows Server, versione 1709

>Si applica a: Windows Server (Canale semestrale)

<img src="../media/landing-icons/new.png" style='float:left; padding:.5em;' alt="Icon showing a newspaper">&nbsp;Per informazioni sulle funzionalità più recenti di Windows, vedi [Novità di Windows Server](whats-new-in-windows-server.md). Questa sezione illustra le funzionalità nuove e modificate di Windows Server, versione 1709. Le nuove funzionalità e modifiche elencate di seguito sono quelle che più probabilmente avranno il massimo impatto sull'uso di questa versione. Vedere anche [Windows Server, versione 1709](https://blogs.technet.microsoft.com/windowsserver/2017/08/24/sneak-peek-1-windows-server-version-1709/).

> [!IMPORTANT]
> Windows Server, versione 1709 non è più supportata a partire dal 9 aprile 2019.


## <a name="new-cadence-of-releases"></a>Nuova frequenza di rilascio

A partire da questa versione, sono disponibili due opzioni per la ricezione degli aggiornamenti delle funzionalità di Windows Server:
- **Long-Term Servicing Channel (LTSC)** : si tratta di un'opzione business, come al solito con 5 anni di supporto Mainstream e 5 anni di supporto Extended. Hai la possibilità di eseguire l'aggiornamento alla versione LTSC successiva ogni 2-3 anni, come supportato negli ultimi 20 anni.
- **Canale semestrale (SAC)** : si tratta di un vantaggio di Software Assurance completamente supportato in ambienti di produzione. La differenza è che è supportato per 18 mesi e sarà disponibile una nuova versione ogni sei mesi.

I canali di rilascio sono riassunti nella tabella seguente.

|   | Canale semestrale | Long-Term Servicing Channel |
| ------------- | ------------- | ------------ |
| Frequenza di rilascio  | Due volte l'anno (primavera e autunno)  | Ogni 2-3 anni |
| Pianificazione supporto  | 18 mesi di supporto in produzione Mainstream  | 5 anni di supporto Mainstream + 5 anni di supporto Extended |
| Disponibilità  | Software Assurance o Azure (ospitato nel cloud)  | Tutti i canali |
| Convenzione di denominazione  | Windows Server, versione AAMM  | Windows Server AAAA |

Per altre informazioni, vedere [Confronto dei canali di manutenzione](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview).

## <a name="application-containers-and-micro-services"></a>Contenitori applicazioni e micro servizi

- L'immagine del contenitore di Server Core è stata ulteriormente ottimizzata per scenari lift-and-shift in cui è possibile eseguire la migrazione di applicazioni o basi codici esistenti in contenitori con modifiche minime. È inoltre il 60% più piccola. 
- L'immagine del contenitore Nano Server è quasi l'80% più piccola.
    - In Canale semestrale di Windows Server, l'immagine Nano Server come un'immagine del sistema operativo di base del contenitore è ridotta da 390 MB a 80 MB.
- Contenitori Linux con isolamento di Hyper-V 

Per altre informazioni, vedere [Modifiche apportate a Nano Server nella versione successiva di Windows Server](https://docs.microsoft.com/windows-server/get-started/nano-in-semi-annual-channel) e [Windows Server, versione 1709 per gli sviluppatori](https://blogs.technet.microsoft.com/windowsserver/2017/09/13/sneak-peek-3-windows-server-version-1709-for-developers/).

## <a name="modern-management"></a>Gestione moderna

Vedere la pagina relativa a [Honolulu](https://docs.microsoft.com/windows-server/manage/honolulu/honolulu) per un'esperienza semplificata, integrata e sicura in grado di consentire agli amministratori IT di gestire scenari di manutenzione, configurazione e risoluzione dei problemi dei componenti di base.  Honolulu include strumenti di ultima generazione con un'interfaccia semplificata, integrata, sicura ed estendibile.
Honolulu include un'esperienza intuitiva completamente nuova per la gestione di PC, istanze di Windows Server, cluster di failover, nonché di un'infrastruttura iperconvergente basata su spazi di archiviazione diretti, riducendo i costi operativi.

## <a name="compute"></a>Calcolo

**Contenitore Nano e contenitore di Server Core**: i primo luogo, questa versione è su come favorire l'innovazione delle applicazioni. Nano Server o Nano come host è deprecato e sostituito dal contenitore Nano, ovvero Nano eseguito come un'immagine del contenitore. 

Per altre informazioni sui contenitori, vedere [Panoramica delle reti contenitore](https://docs.microsoft.com/windows-server/networking/sdn/technologies/containers/container-networking-overview).

L'host **Server Core come contenitore** (e infrastruttura) offre flessibilità, densità e prestazioni migliori per le applicazioni esistenti in un processo di modernizzazione migliori e nuove app sviluppate usando già il modello di cloud.

L'**ordine di avvio per le macchine virtuali** è stato migliorato anche con il riconoscimento del sistema operativo e dell'applicazione, con trigger ottimizzati per i casi in cui VM viene considerata come avviata prima dell'avvio della VM successiva.

Il **supporto della memoria della classe di archiviazione per macchine virtuali** consente di creare volumi NTFS ad accesso diretto in DIMM non volatili e di esporli in macchine virtuali Hyper-V. In questo modo le macchine virtuali Hyper-V possono sfruttare i vantaggi delle prestazioni a bassa latenza dei dispositivi di memoria della classe di archiviazione.

La **memoria persistente virtualizzata (vPMEM, Virtualized Persistent Memory)** è abilitata tramite la creazione di un file VHD (con estensione vhdpmem) su un volume ad accesso diretto in un host, l'aggiunta di un controller vPMEM a una macchina virtuale e l'aggiunta del dispositivo creato (con estensione vhdpmem) a una macchina virtuale. L'uso del file vhdpmem sui volumi ad accesso diretto in un host per eseguire il backup di vPMEM offre flessibilità di allocazione e sfrutta un modello di gestione familiare per l'aggiunta di dischi alle macchine virtuali.

**Archiviazione contenitori, volumi di dati persistenti in volumi condivisi cluster (CSV)** . In Windows Server versione 1709, nonché in Windows Server 2016 con gli aggiornamenti più recenti è stato aggiunto il supporto che consente ai contenitori di accedere ai volumi di dati persistenti in volumi condivisi cluster, inclusi i volumi condivisi cluster in Spazi di archiviazione diretta. In questo modo, il contenitore applicazione dispone di accesso permanente al volume, indipendentemente dal nodo cluster su cui è in esecuzione l'istanza del contenitore. Per altre informazioni, vedere la pagina relativa al [supporto di archiviazione contenitore con volumi condivisi cluster (CSV), spazi di archiviazione diretta (S2D), mapping globale SMB](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/).

**Archiviazione contenitori, volumi di dati persistenti con mapping globale SMB**. In Windows Server versione 1709 è stato aggiunto il supporto per il mapping di una condivisione file SMB a una lettera di unità all'interno di un contenitore. Ciò è noto con il nome di mapping globale SMB. Questa unità mappata è quindi accessibile a tutti gli utenti nel server locale in modo che l'I/O del contenitore nel volume di dati possa passare dall'unità montata alla condivisione di file sottostante. Per altre informazioni, vedere la pagina relativa al [supporto di archiviazione contenitore con volumi condivisi cluster (CSV), spazi di archiviazione diretta (S2D), mapping globale SMB](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/).

**Formato di file di configurazione della macchina virtuale (aggiornato)** . È stato aggiunto un ulteriore file (con estensione vmgs) per le macchine virtuali con una versione di configurazione 8.2 e versioni successive. VMGS è l'acronimo di "VM guest" (guest VM) ed è un nuovo file interno che include lo stato del dispositivo che in precedenza faceva parte del file di stato di runtime della macchina virtuale.

## <a name="security-and-assurance"></a>Sicurezza e controllo

La **codifica di rete** consente di codificare rapidamente i segmenti di rete su un'infrastruttura di rete software-defined per soddisfare le esigenze di sicurezza e conformità.

Il **servizio Sorveglianza host (HGS)** come macchina virtuale schermata è abilitato. Prima di questo rilascio, si consigliava di distribuire un cluster fisico di 3 nodi. Sebbene ciò garantisca che l'ambiente HGS non venga compromesso da un amministratore, tale azione richiede in genere costi proibitivi.

**Linux come una macchina virtuale schermata** è ora supportato.

Per altre informazioni, vedere la [panoramica dell'infrastruttura sorvegliata e delle macchine virtuali schermate](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms).

**Vulnerabilità SMBLoris**: è stato risolto il problema noto come "SMBLoris", che potrebbe causare un attacco Denial of Service.

## <a name="storage"></a>Archiviazione

**Replica archiviazione**: la protezione del ripristino di emergenza aggiunta tramite Replica di archiviazione in Windows Server 2016 è ora estesa per includere:
- **Failover di test**: l'opzione per montare l'archiviazione di destinazione è ora possibile tramite la funzionalità di failover di test. È possibile montare temporaneamente uno snapshot dell'archiviazione replicata nei nodi di destinazione a scopo di test o backup.  Per altre informazioni, vedi [Domande frequenti su Replica di archiviazione](https://aka.ms/srfaq). 
- **Supporto di Honolulu**: il supporto per la gestione grafica della replica da server a server è ora disponibile in Honolulu. In questo modo, non è più necessario usare PowerShell per gestire un carico di lavoro di protezione in caso di emergenza.

**SMB**: 
- **Rimozione dell'autenticazione guest e SMB1**: Windows Server, versione 1709 non esegue più l'installazione del server e del client SMB1 per impostazione predefinita. Inoltre, la possibilità di eseguire l'autenticazione come Guest in SMB2 e versioni successive è disattivata per impostazione predefinita. Per altre informazioni, leggere l'articolo in cui è indicato che [SMBv1 non è installato per impostazione predefinita in Windows 10 versione 1709 e Windows Server versione 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server). 

- **Compatibilità e sicurezza SMB2/SMB3**: sono state aggiunte ulteriori opzioni per la compatibilità delle applicazioni e la sicurezza, inclusa la possibilità di disabilitare oplock in SMB2+ per le applicazioni legacy e di richiedere la firma o codifica per ogni connessione da un client. Per altre informazioni, consultare la guida del modulo SMBShare PowerShell.

**Deduplicazione dati**: 
- **Deduplicazione dati supporta ora ReFS**: non è più necessario scegliere tra i vantaggi di un file system moderno con ReFS e Deduplicazione dati. È ora possibile abilitare Deduplicazione dati ovunque sia possibile abilitare ReFS. Ciò comporta un aumento dell'efficienza di archiviazione di oltre il 95% con ReFS.
- **API DataPort per traffico in ingresso/uscita ottimizzato ai volumi deduplicati**: gli sviluppatori possono ora sfruttare le informazioni di cui dispone Deduplicazione dati su come archiviare dati in modo efficiente per spostarli tra server, volumi e cluster in modo efficiente.

## <a name="remote-desktop-services-rds"></a>Servizi Desktop remoto (RDS)

**Servizi Desktop remoto è integrato con Azure AD**, in modo da consentire ai clienti di sfruttare i criteri di accesso condizionale, l'autenticazione a più fattori, l'autenticazione integrata con altre app SaaS utilizzando Azure AD e altro ancora. Per altre informazioni, vedere l'articolo relativo all'[integrazione di Azure AD Domain Services con la distribuzione di Servizi Desktop remoto](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-azure-adds).

>[!TIP]
>Per dare un'occhiata in anteprima alle altre straordinarie modifiche in arrivo per Servizi Desktop remoto, vedere [Servizi Desktop remoto: aggiornamenti e innovazioni future](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/20/first-look-at-updates-coming-to-remote-desktop-services/)

## <a name="networking"></a>Rete

La **mesh di routing di Docker** è supportata. La mesh di routing in ingresso fa parte della [modalità Swarm](https://docs.docker.com/engine/swarm/), soluzione di orchestrazione incorporata di Docker per i contenitori. Per altre informazioni, vedere la pagina relativa alla [disponibilità della mesh di routing in Windows Server versione 1709](https://blogs.technet.microsoft.com/virtualization/2017/09/26/dockers-ingress-routing-mesh-available-with-windows-server-version-1709/).

**Nuove funzionalità per Docker** sono disponibili. Per altre informazioni, vedere la pagina relativa a [interessanti novità per Docker con Windows Server 1709](https://blog.docker.com/2017/09/docker-windows-server-1709/).

**Reti di Windows alla parità con Linux per Kubernetes**: Windows è ora alla pari con Linux in termini di reti. I clienti possono distribuire cluster Kubernetes con sistemi operativi misti in qualsiasi ambiente, inclusi Azure, in locale e in stack cloud di terzi con le stesse topologie e primitive di rete supportate su Linux, senza la necessità di eventuali soluzioni alternative o le estensioni del commutatore.

**Stack di rete core**: sono state migliorate numerose funzionalità dello stack di rete core. Per altre informazioni su queste funzionalità, vedere la pagina relativa alle [funzionalità dello stack di rete core in Windows 10 Creators Update](https://blogs.technet.microsoft.com/networking/2017/07/13/core-network-stack-features-in-the-creators-update-for-windows-10/).
- **TCP Fast Open (TFO)** : è stato aggiunto il supporto per TFO al fine di ottimizzare il processo di handshake a 3 vie TCP. TFO definisce un cookie TFO sicuro alla prima connessione tramite un handshake a 3 vie standard.  Le connessioni successive allo stesso server utilizzano il cookie TFO anziché un handshake a 3 vie per connettersi con un tempo di round trip pari a zero.
- **CUBIC**: è disponibile un'implementazione nativa di Windows sperimentale di CUBIC, un algoritmo di controllo della congestione TCP. I comandi seguenti consentono rispettivamente di abilitare o disabilitare CUBIC.

    ```
    netsh int tcp set supplemental template=internet congestionprovider=cubic
    netsh int tcp set supplemental template=internet congestionprovider=compound
    ```

- **Regolazione automatica della finestra di ricezione**: la logica di regolazione automatica TCP calcola il parametro della "finestra di ricezione" di una connessione TCP.  Le connessioni ad alta velocità e/o a ritardo lungo richiedono questo algoritmo per ottenere caratteristiche prestazioni ottimali.  In questa versione, l'algoritmo è modificato in modo da usare una funzione a gradino per la convergenza su un valore di finestra di ricezione massimo per una determinata connessione.
- **API statistiche TCP**: è stata introdotta una nuova API denominata SIO_TCP_INFO.  SIO_TCP_INFO consente agli sviluppatori di eseguire query di informazioni avanzate sulle singole connessioni TCP utilizzando un'opzione socket.
- **IPv6**: in questo rilascio sono disponibili numerosi miglioramenti in IPv6.
  - Supporto di **RFC 6106**: RFC 6106 consente la configurazione DNS tramite annunci router (RA). Per abilitare o disabilitare il supporto di RFC 6106, puoi utilizzare il comando seguente:

    ```
    netsh int ipv6 set interface <ifindex> rabaseddnsconfig=<enabled | disabled>
    ```

  - **Etichette di flusso**: a partire da Creators Update, questo campo è impostato su un hash a 5 tuple (IP origine, IP destinazione, porta origine, porta destinazione) per i pacchetti TCP e UDP in uscita su IPv6.  In questo modo, il bilanciamento del carico o la classificazione del flusso è più efficiente per i data center solo IPv6. Per abilitare le etichette di flusso:

    ```
    netsh int ipv6 set flowlabel=[disabled|enabled] (enabled by default)
    netsh int ipv6 set global flowlabel=<enabled | disabled>
    ```

  - **ISATAP e 6to4**: in Creators Update queste tecnologie sono disabilitate per impostazione predefinita, visto che saranno deprecate in futuro.
- **Rilevamento Dead Gateway (DGD)** : l'algoritmo di rilevamento Dead Gateway esegue automaticamente la transizione delle connessioni a un altro gateway quando il gateway corrente non è raggiungibile. In questo rilascio l'algoritmo è stato migliorato per eseguire di nuovo il probe dell'ambiente di rete periodicamente.
- [Test-NetConnection](https://technet.microsoft.com/itpro/powershell/windows/nettcpip/test-netconnection) è un cmdlet incorporato in Windows PowerShell che esegue un'ampia gamma di diagnostica di rete.  In questa versione abbiamo migliorato il cmdlet per fornire informazioni dettagliate sulla selezione delle route e sulla selezione dell'indirizzo di origine.

**SDN (Software Defined Networking)**

- **Codifica di rete virtuale** è una nuova funzionalità che offre la possibilità di codificare il traffico di rete virtuale tra le macchine virtuali che comunicano tra loro all'interno di subnet contrassegnate come "Crittografia abilitata". Questa funzionalità usa Datagram Transport Layer Security (DTLS) nella subnet virtuale per codificare i pacchetti.  DTLS fornisce protezione contro intercettazione, manomissione e contraffazione da chiunque abbia accesso alla rete fisica.
 
**Windows 10 VPN**

- **Tunnel dell'infrastruttura pre-accesso**. Per impostazione predefinita, la VPN di Windows 10 non crea automaticamente i tunnel dell'infrastruttura quando gli utenti non sono connessi al computer o dispositivo. È possibile configurare la VPN di Windows 10 per creare automaticamente i tunnel dell'infrastruttura pre-accesso tramite la funzionalità relativa al tunnel dei dispositivi (pre-accesso) nel profilo della VPN.
- **Gestione di dispositivi e computer remoti**.  Puoi gestire i client VPN di Windows 10 configurando la funzionalità relativa al tunnel dei dispositivi (pre-accesso) nel profilo della VPN. Devi inoltre configurare la connessione VPN per registrare in modo dinamico gli indirizzi IP assegnati all'interfaccia VPN con i servizi DNS interni.
- **Specificare i gateway di pre-accesso**. Puoi specificare i gateway di pre-accesso con la funzionalità relativa al tunnel dei dispositivi (pre-accesso) nel profilo della VPN, combinata con i filtri di traffico per controllare quali sistemi di gestione della rete aziendale sono accessibili tramite il tunnel dei dispositivi.

