---
title: Novità di Windows Server 2019
description: Panoramica delle nuove funzionalità in Windows Server 2019, tra cui Esperienza desktop, il servizio di migrazione archiviazione, informazioni dettagliate di sistema, la scheda di rete di Azure, i miglioramenti a Spazi di archiviazione diretta e altre modifiche.
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: high
ms.date: 06/04/2019
ms.openlocfilehash: 13eed225dfc144d5e7e59be13dbed14d4de8bb01
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360784"
---
# <a name="whats-new-in-windows-server-2019"></a>Novità di Windows Server 2019

> Si applica a: Windows Server 2019

Questo argomento descrive alcune delle nuove funzionalità di Windows Server 2019. Windows Server 2019 si basa sulle solide fondamenta di Windows Server 2016 e include numerose innovazioni su quattro temi chiave: cloud ibrido, sicurezza, piattaforma per applicazioni e infrastruttura iperconvergente (HCI).

Per scoprire le novità dei rilasci di Canale semestrale per Windows Server, vedi [Novità di Windows Server](../get-started/whats-new-in-windows-server.md).

## <a name="general"></a>Generale

### <a name="windows-admin-center"></a>Windows Admin Center

Windows Admin Center è un'app distribuita in locale e basata su browser che consente di gestire server, cluster, infrastruttura iperconvergente e PC Windows 10. Non prevede costi aggiuntivi rispetto a Windows ed è pronta per essere usata in produzione.

È possibile installare Windows Admin Center in Windows Server 2019, oltre che in Windows 10 e versioni precedenti di Windows e Windows Server, e usarlo per gestire server e cluster che eseguono Windows Server 2008 R2 e versioni successive.

Per altre informazioni, vedi [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md).

### <a name="desktop-experience"></a>Esperienza desktop

Poiché Windows Server 2019 è un rilascio di LTSC (Long-Term Servicing Channel), include <b>Esperienza desktop</b>. I rilasci di Canale semestrale \(SAC\) non includono Esperienza desktop per impostazione predefinita. Sono strettamente correlati ai rilasci delle immagini di contenitore Server Core e Nano Server. Come per Windows Server 2016, durante l'installazione del sistema operativo puoi scegliere tra le installazioni Server Core o Server con Esperienza desktop.

### <a name="system-insights"></a>Informazioni dettagliate di sistema

Informazioni dettagliate di sistema è una nuova funzione disponibile in Windows Server 2019 che offre funzionalità di analisi predittiva locale in modalità nativa per Windows Server. Queste funzionalità predittive, ognuna abbinata a un modello di apprendimento automatico, analizzano localmente i dati di sistema di Windows Server, ad esempio i contatori delle prestazioni e gli eventi, fornendo dati approfonditi in merito al funzionamento dei server e contribuendo a ridurre i costi operativi associati alla gestione reattiva delle problematiche di Windows Server.

## <a name="hybrid-cloud"></a>Cloud ibrido

### <a name="server-core-app-compatibility-feature-on-demand"></a>Funzionalità di compatibilità dell'app Server Core su richiesta

La [funzionalità di compatibilità dell'app Server Core su richiesta](https://docs.microsoft.com/windows-server/get-started-19/install-fod-19) migliora notevolmente la compatibilità dell'app dell'opzione di installazione dei componenti di base di Windows Server attraverso l'inclusione di un subset di file binari e componenti di Windows Server con Esperienza desktop, senza l'aggiunta dell'ambiente grafico di Esperienza desktop di Windows Server.  Questo ha lo scopo di migliorare la funzionalità e la compatibilità di Server Core, mantenendolo comunque il più snello possibile.  

Questa funzionalità su richiesta facoltativa è disponibile in un file ISO separato e può essere aggiunta solo alle immagini e alle installazioni dei componenti di base di Windows Server, tramite Gestione e manutenzione immagini distribuzione. 

## <a name="security"></a>Sicurezza

### <a name="windows-defender-advanced-threat-protection-atp"></a>Windows Defender Advanced Threat Protection (ATP)

Sensori avanzati della piattaforma e azioni di risposta di ATP espongono agli attacchi a livello di memoria e kernel e rispondono eliminando file dannosi e terminando processi dannosi.

-   Per altre informazioni su Windows Defender ATP, vedi [Panoramica delle funzionalità di Windows Defender ATP](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview).

-   Per altre informazioni sull'onboarding di server, vedi [Eseguire l'onboarding dei server al servizio Windows Defender ATP](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection).

**Windows Defender ATP Exploit Guard** è un nuovo gruppo di funzionalità di prevenzione delle intrusioni. I quattro componenti di Windows Defender Exploit Guard sono progettati per proteggere il dispositivo da un'ampia gamma di vettori di attacco e comportamenti di blocco comunemente usati in attacchi di malware, consentendoti di trovare il giusto equilibrio tra rischi di sicurezza e requisiti di produttività.

-   [Riduzione della superficie di attacco](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/attack-surface-reduction-exploit-guard?ocid=cx-blog-mmpc) è un gruppo di controlli che le grandi imprese possono abilitare per impedire l'accesso al computer di malware attraverso il blocco di file dannosi sospetti (ad esempio, file di Office), script, spostamento laterale, comportamento ransomware e minacce basate sulla posta elettronica.

-   [Protezione di rete](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/network-protection-exploit-guard?ocid=cx-blog-mmpc) protegge l'endpoint da minacce basate sul Web bloccando qualsiasi processo in uscita nel dispositivo per gli indirizzi host o IP non attendibili tramite Windows Defender SmartScreen.

-   [Accesso controllato alle cartelle](https://cloudblogs.microsoft.com/microsoftsecure/2017/10/23/stopping-ransomware-where-it-counts-protecting-your-data-with-controlled-folder-access/?ocid=cx-blog-mmpc?source=mmpc) protegge i dati sensibili da ransomware impedendo l'accesso di processi non attendibili alle cartelle protette.

-   [Protezione dagli exploit](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/exploit-protection-exploit-guard) è un gruppo di misure di prevenzione per gli exploit di vulnerabilità (in sostituzione di EMET) che puoi configurare facilmente per proteggere il sistema e le applicazioni.

[Controllo di applicazioni di Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control) (noto anche come criteri di integrità del codice) è stato rilasciato in Windows Server 2016.
Sulla base del feedback fornito dai clienti, l'idea alla base si questa funzionalità è ottima, ma difficile da implementare.
Per risolvere questo problema, abbiamo creato criteri di integrazione continua predefiniti, che consentono a tutti i file interni di Windows e alle applicazioni Microsoft, ad esempio SQL Server, di bloccare i file eseguibili noti che possono ignorare l'integrazione continua. 

### <a name="security-with-software-defined-networking-sdn"></a>Sicurezza con Software Defined Networking (SDN)

[Sicurezza con SDN](https://docs.microsoft.com/windows-server/networking/sdn/security/sdn-security-top) offre numerose funzionalità per aumentare la fiducia dei clienti che eseguono carichi di lavoro, in locale, o come provider di servizi nel cloud. 

Questi miglioramenti apportati alla sicurezza sono integrati nella piattaforma SDN completa introdotta in Windows Server 2016.

Per un elenco completo delle novità di SDN, vedi [Novità di SDN per Windows Server 2019](https://docs.microsoft.com/windows-server/networking/sdn/sdn-whats-new).

### <a name="shielded-virtual-machines-improvements"></a>Miglioramenti alle macchine virtuali schermate

- **Miglioramenti di succursale**

    Ora puoi eseguire macchine virtuali schermate in computer con connettività intermittente al servizio Sorveglianza host sfruttando le nuove funzionalità del [server HGS di fallback](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#fallback-configuration) e la [modalità offline](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#offline-mode). Il server HGS di fallback ti consente di configurare un secondo gruppo di URL per Hyper-V per provare se è in grado di raggiungere il server HGS primario.

    La modalità offline ti consente di continuare ad avviare le macchine virtuali schermate, anche se HGS non può essere raggiunto, purché la macchina virtuale sia stata avviata correttamente una volta e la configurazione della sicurezza dell'host non sia cambiata.

- **Miglioramenti alla risoluzione dei problemi**

    Abbiamo anche semplificato la [risoluzione dei problemi delle macchine virtuali schermate](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms) abilitando il supporto per la modalità sessione avanzata VMConnect e PowerShell Direct. Questi strumenti sono particolarmente utili se hai perso la connettività di rete alla macchina virtuale e devi aggiornarne la configurazione per ripristinare l'accesso. 

    Queste funzionalità non devono essere configurate e vengono rese disponibili automaticamente quando una macchina virtuale schermata viene posizionata su un host Hyper-V che esegue Windows Server versione 1803 o successiva.

- **Supporto di Linux**

    Se esegui ambienti di sistema operativo misto, adesso Windows Server 2019 supporta l'esecuzione di Ubuntu, Red Hat Enterprise Linux e SUSE Linux Enterprise Server in macchine virtuali schermate.

### <a name="http2-for-a-faster-and-safer-web"></a>HTTP/2 per un Web più veloce e sicuro

- Unione delle connessioni migliorata per un'esperienza di esplorazione senza interruzioni e correttamente crittografata.

- Negoziazione del pacchetto di crittografia lato server di HTTP/2 aggiornata per la prevenzione automatica di errori di connessione e facilità di distribuzione.

- Come provider di congestione TCP predefinito è stato configurato Cubic per offrire una maggiore velocità effettiva.

## <a name="storage"></a>Archiviazione

Ecco alcune delle modifiche apportate all'archiviazione in Windows Server 2019. Per altre informazioni, vedi [Novità nell'archiviazione](../storage/whats-new-in-storage.md).

### <a name="storage-migration-service"></a>Servizio di migrazione della risorsa di archiviazione

Il servizio di migrazione archiviazione è una nuova tecnologia che semplifica la migrazione dei server a una versione più recente di Windows Server. Fornisce uno strumento grafico che esegue l'inventario dei dati nei server, trasferisce i dati e la configurazione nei server più recenti e, facoltativamente, sposta le identità dei server precedenti ai nuovi server in modo che le app e gli utenti non debbano apportare alcuna modifica. Per altre informazioni, vedi [Servizio di migrazione archiviazione](../storage/storage-migration-service/overview.md).

### <a name="storage-spaces-direct"></a>Spazi di archiviazione diretta

Ecco un elenco delle novità in Spazi di archiviazione diretta. Per informazioni dettagliate, vedi [Novità in Spazi di archiviazione diretta](../storage/whats-new-in-storage.md#storage-spaces-direct). Vedi anche [Azure Stack HCI](https://docs.microsoft.com/azure-stack/operator/azure-stack-hci-overview) per informazioni sull'acquisizione di sistemi convalidati di Spazi di archiviazione diretta.

- **Deduplicazione e compressione dei volumi ReFS**
- **Supporto nativo per la memoria persistente**
- **Resilienza annidata per l'infrastruttura iperconvergente a due nodi periferica**
- **Cluster a due server con un'unità flash USB di controllo**
- **Supporto di Windows Admin Center**
- **Cronologia delle prestazioni**
- **Scalabilità fino a 4 PB per ogni cluster**
- **Parità accelerata con mirroring 2 volte più veloce**
- **Rilevamento dell'outlier con latenza unità**
- **Delimitazione manuale dell'allocazione dei volumi per aumentare la tolleranza di errore**

### <a name="storage-replica"></a>Replica archiviazione

Ecco le novità introdotte in Replica di archiviazione. Per informazioni dettagliate, vedi [Novità in Replica di archiviazione](../storage/whats-new-in-storage.md#storage-replica).

- Replica di archiviazione è ora disponibile in Windows Server 2019 Standard Edition.
- Il failover di test è una nuova funzionalità che consente il montaggio dell'archiviazione di destinazione per convalidare i dati di replica o di backup. Per altre informazioni, vedi [Domande frequenti su Replica di archiviazione](../storage/storage-replica/storage-replica-frequently-asked-questions.md).
- Miglioramenti alle prestazioni del log per Replica di archiviazione
- Supporto di Windows Admin Center

## <a name="failover-clustering"></a>Clustering di failover

Ecco un elenco delle novità in Clustering di failover. Per informazioni dettagliate, vedi [Novità in Clustering di failover](../failover-clustering/whats-new-in-failover-clustering.md).

- **Set di cluster**
- **Cluster compatibili con Azure**
- **Migrazione di cluster tra domini**
- **Controllo USB**
- **Miglioramenti all'infrastruttura cluster**
- **L'aggiornamento compatibile con i cluster supporta Spazi di archiviazione diretta**
- **Miglioramenti al controllo della condivisione file**
- **Protezione avanzata del cluster**
- **Il cluster di failover non usa più l'autenticazione NTLM**

## <a name="application-platform"></a>Piattaforma per applicazioni

### <a name="linux-containers-on-windows"></a>Contenitori di Linux in Windows

È ora possibile eseguire contenitori basati su Linux e Windows nello stesso host contenitore usando lo stesso daemon docker. Ciò consente di disporre di un ambiente costituito da host contenitori eterogenei e offre al tempo stesso flessibilità agli sviluppatori di applicazioni.

### <a name="built-in-support-for-kubernetes"></a>Supporto incorporato per Kubernetes

Windows Server 2019 prosegue i miglioramenti in fatto di elaborazione, rete e archiviazione introdotti dai rilasci di Canale semestrale, necessari per supportare Kubernetes su Windows. Altri dettagli sono disponibili nelle versioni future di Kubernetes.

- [Rete di contenitori](https://docs.microsoft.com/windows-server/networking/sdn/technologies/containers/container-networking-overview) in Windows Server 2019 aumenta notevolmente l'usabilità di Kubernetes in Windows migliorando la resilienza di rete della piattaforma e il supporto di plug-in della rete dei contenitori.

- I carichi di lavoro distribuiti su Kubernetes sono in grado di usare la sicurezza di rete per proteggere i servizi di Linux e Windows con strumenti incorporati.

### <a name="container-improvements"></a>Miglioramenti ai contenitori
    
- **Identità integrata avanzata**

    L'autenticazione integrata di Windows nei contenitori è stata resa più semplice e affidabile, risolvendo così diverse limitazioni rispetto alle versioni precedenti di Windows Server.

- **Migliore compatibilità delle applicazioni**

    La containerizzazione di applicazioni basate su Windows è ora più semplice. La compatibilità delle app per l'immagine *windowsservercore* esistente è stata migliorata. Per le applicazioni con dipendenze di API aggiuntive, è ora disponibile una terza immagine di base: *windows*.

- **Dimensioni ridotte e prestazioni superiori**

    Le dimensioni di download dell'immagine contenitore di base, la dimensione su disco e i tempi di avvio sono stati migliorati. Questo accelera i flussi di lavoro del contenitore

- **Esperienza di gestione con Windows Admin Center \(preview\)**

    Vedere quali contenitori sono in esecuzione nel computer e gestire i singoli contenitori con una nuova estensione per Windows Admin Center non è mai stato così semplice. Cerca l'estensione "Containers" nel [feed pubblico di Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/using-extensions).

### <a name="encrypted-networks"></a>Reti crittografate

[Reti codificate](https://docs.microsoft.com/windows-server/networking/sdn/sdn-whats-new): la codifica di rete virtuale consente la codifica del traffico di rete virtuale tra le macchine virtuali che comunicano tra loro all'interno di subnet contrassegnate come **Codifica abilitata**. Viene inoltre utilizzato Datagram Transport Layer Security (DTLS) nella subnet virtuale per crittografare i pacchetti. DTLS impedisce intercettazioni, manomissioni e contraffazioni da parte di chiunque abbia accesso alla rete fisica.

### <a name="network-performance-improvements-for-virtual-workloads"></a>Miglioramenti alle prestazioni di rete per i carichi di lavoro virtuali

I [miglioramenti alle prestazioni di rete per i carichi di lavoro virtuali](https://docs.microsoft.com/windows-server/networking/technologies/hpn/hpn-insider-preview) consentono di ottimizzare la velocità effettiva di rete sulle macchine virtuali senza dover costantemente sincronizzare o effettuare il provisioning eccessivo dell'host. Questo riduce le operazioni e i costi di manutenzione, migliorando la densità disponibile degli host. Queste nuove funzionalità includono:

* Unione segmenti ricevuti (RSC) nel commutatore virtuale

* Coda di più macchine virtuali dinamica (d.VMMQ)

### <a name="low-extra-delay-background-transport"></a>Low Extra Delay Background Transport

Low Extra Delay Background Transport (LEDBAT) è un provider di controllo della congestione della rete, ottimizzato per la latenza, che è stato progettato per concedere automaticamente larghezza di banda a utenti e applicazioni e utilizzare l'intera larghezza di banda disponibile quando la rete non è in uso.   
Questa tecnologia è stata progettata per l'uso nella distribuzione di aggiornamenti critici di grandi dimensioni in un ambiente IT senza influire sui servizi per i clienti e sulla larghezza di banda associata.

### <a name="windows-time-service"></a>Ora di Windows

Il [servizio Ora di Windows](https://docs.microsoft.com/windows-server/networking/windows-time-service/insider-preview) include un reale supporto di secondi intercalari conforme a UTC, un nuovo protocollo orario chiamato Precision Time Protocol e tracciabilità end-to-end.


### <a name="high-performance-sdn-gateways"></a>Gateway SDN ad alte prestazioni

[I gateway SDN ad alte prestazioni](https://docs.microsoft.com/windows-server/networking/sdn/gateway-performance) in Windows Server 2019 migliorano notevolmente le prestazioni per le connessioni IPsec e GRE, fornendo una velocità effettiva molto elevata con un utilizzo della CPU molto inferiore.
<br/>

### <a name="new-deployment-ui-and-windows-admin-center-extension-for-sdn"></a>Nuova estensione dell'interfaccia di distribuzione e di Windows Admin Center per SDN

Con Windows Server 2019, è facile ora eseguire operazioni di distribuzione e gestione tramite una nuova interfaccia utente di sviluppo e un'estensione di Windows Admin Center che consentono a tutti di sfruttare le potenzialità di SDN. 

### <a name="persistent-memory-support-for-hyper-v-vms"></a>Supporto della memoria persistente per macchine virtuali Hyper-V

Per sfruttare la velocità effettiva elevata e la bassa latenza della memoria persistente (ovvero la memoria della classe di archiviazione) nelle macchine virtuali, adesso la memoria può essere proiettata direttamente nelle macchine virtuali. Ciò consente di ridurre notevolmente la latenza di transazione del database o i tempi di ripristino per i database in memoria a bassa latenza in caso di errore.

