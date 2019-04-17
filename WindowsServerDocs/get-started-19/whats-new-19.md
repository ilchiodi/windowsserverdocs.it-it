---
title: Novità di Windows Server 2019
description: Panoramica delle nuove funzionalità in Windows Server 2019, tra cui Esperienza desktop, il servizio di migrazione della risorsa di archiviazione, le informazioni dettagliate di sistema, la scheda di rete di Azure, miglioramenti agli Spazi di archiviazione diretta e altre modifiche.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: high
ms.openlocfilehash: 4c454fc397b662e313d5cfb7ed02a83dc7059207
ms.sourcegitcommit: e73fbe1046a8bd2bf4f24ccffc11465ad8dfab1d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/07/2019
ms.locfileid: "8992544"
---
# Novità di Windows Server 2019

In questo argomento vengono descritte alcune delle nuove funzionalità di Windows Server 2019. Windows Server 2019 si basa sulle solide fondamenta di Windows Server 2016 e offre numerose innovazioni su quattro temi chiave: cloud ibrido, sicurezza, piattaforma per applicazioni e infrastruttura iperconvergente (HCI). Per scoprire le novità in Windows Server, versione 1809, vedi [Novità di Windows Server, versione 1809](../get-started/whats-new-in-windows-server-1809.md).

## Generale

### Esperienza desktop

Poiché Windows Server 2019 è una versione LTSC (Long-Term Servicing Channel), include l'<b>esperienza Desktop</b>. (non è inclusa in Windows Server, versione 1709, Windows Server, versione 1803 o Windows Server, versione 1809, poiché le versioni con canale semestrale \(SAC\) che non includono l'esperienza desktop per impostazione predefinita sono strettamente le versioni Server Core e Server Nano). Come in Windows Server 2016, durante l'installazione del sistema operativo è possibile scegliere tra le installazioni Server Core o Server con installazioni con esperienza Desktop.

### Informazioni dettagliate di sistema

Informazioni dettagliate di sistema è una nuova funzione disponibile in Windows Server 2019 che offre funzionalità di analisi predittiva locale in modalità nativa per Windows Server. Queste funzionalità predittive, ognuna abbinata a un modello di apprendimento automatico, analizzano localmente i dati di sistema di Windows Server, ad esempio i contatori delle prestazioni e gli eventi, fornendo dati approfonditi in merito al funzionamento dei server e contribuendo a ridurre i costi operativi associati alla gestione reattiva delle problematiche di Windows Server.

## Cloud ibrido

### Funzionalità di compatibilità dell'app Server Core su richiesta

La [funzionalità di compatibilità dell'app Server Core su richiesta](https://docs.microsoft.com/windows-server/get-started-19/install-fod-19) migliora notevolmente la compatibilità dell'app dell'opzione di installazione dei componenti di base di Windows Server attraverso l'inclusione di un sottoinsieme di file binari e componenti di Windows Server con Esperienza desktop, senza l'aggiunta dell'ambiente grafico di Esperienza desktop di Windows Server.  Questo ha lo scopo di migliorare la funzionalità e la compatibilità di Server Core, mantenendolo comunque più snello possibile.  

Questa funzionalità su richiesta facoltativa è disponibile in un file ISO separato e può essere aggiunta solo alle immagini e alle installazioni di base di Windows Server, mediante Gestione e manutenzione immagini distribuzione. 

## Sicurezza

### Windows Defender Advanced Threat Protection (ATP)

Sensori avanzati della piattaforma e azioni di risposta di ATP espongono agli attacchi a livello di memoria e kernel e rispondono eliminando file dannosi e terminando processi dannosi.

-   Per altre informazioni su Windows Defender ATP, vedi [Panoramica delle funzionalità di Windows Defender ATP](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview).

-   Per ulteriori informazioni sull'onboarding di server, vedi [Eseguire l'onboarding dei server al servizio Windows Defender ATP](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection).

**Windows Defender ATP Exploit Guard** è un nuovo gruppo di funzionalità di prevenzione delle intrusioni. I quattro componenti di Windows Defender Exploit Guard sono progettati per proteggere il dispositivo da un'ampia gamma di vettori di attacco e comportamenti di blocco comunemente utilizzati in attacchi di malware, consentendoti di trovare il giusto equilibrio tra rischi correlati alla sicurezza e requisiti di produttività.

-   [Riduzione della superficie di attacco](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/attack-surface-reduction-exploit-guard?ocid=cx-blog-mmpc) è un gruppo di controlli che le grandi imprese possono abilitare per impedire l'accesso al computer di malware attraverso il blocco di file dannosi sospetti (ad esempio, file di Office), script, spostamento laterale, comportamento ransomware e minacce basate sulla posta elettronica.

-   [Protezione di rete](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/network-protection-exploit-guard?ocid=cx-blog-mmpc) protegge l'endpoint da minacce basate sul Web bloccando qualsiasi processo in uscita nel dispositivo per gli indirizzi host o IP non attendibili tramite Windows Defender SmartScreen.

-   [Accesso controllato alle cartelle](https://cloudblogs.microsoft.com/microsoftsecure/2017/10/23/stopping-ransomware-where-it-counts-protecting-your-data-with-controlled-folder-access/?ocid=cx-blog-mmpc?source=mmpc) protegge i dati sensibili da ransomware impedendo l'accesso di processi non attendibili alle cartelle protette.

-   [Protezione dagli exploit](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/exploit-protection-exploit-guard) è un gruppo di misure di prevenzione per gli exploit di vulnerabilità (in sostituzione di EMET) che è possibile configurare facilmente per proteggere il sistema e le applicazioni.



[Controllo di applicazioni di Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control) (noto anche come criteri di integrità del codice) è stato rilasciato in Windows Server 2016.
Sulla base dei feedback forniti dai clienti l'idea è ottima, ma difficile da implementare.
Per risolvere questo problema, abbiamo creato criteri di integrazione continua predefiniti, che consentono a tutti i file interni di Windows e alle applicazioni Microsoft, ad esempio SQL Server, di bloccare i file eseguibili noti che possono ignorare l'integrazione continua. 

### Sicurezza con Software Defined Networking (SDN)

[Sicurezza con SDN](https://docs.microsoft.com/windows-server/networking/sdn/security/sdn-security-top) offre numerose funzionalità per aumentare la fiducia dei clienti che eseguono carichi di lavoro, in locale, o come provider di servizi nel cloud. 

Questi miglioramenti della sicurezza sono integrati nella piattaforma SDN completa introdotta in Windows Server 2016.

Per un elenco completo delle novità di SDN, vedi [Novità di SDN per Windows Server 2019](https://docs.microsoft.com/windows-server/networking/sdn/sdn-whats-new).

### Miglioramenti delle macchine virtuali schermate

- **Miglioramenti di succursale**

    Ora puoi eseguire macchine virtuali schermate in computer con connettività intermittente al servizio Sorveglianza host sfruttando le nuove funzionalità del [server HGS di fallback](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#fallback-configuration) e la [modalità offline](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#offline-mode). Il server HGS di fallback ti consente di configurare un secondo gruppo di URL per Hyper-V per provare se è in grado di raggiungere il server HGS primario.

    La modalità offline ti consente di continuare ad avviare le macchine virtuali schermate, anche se HGS non può essere raggiunto, purché la macchina virtuale sia stata avviata correttamente una volta e la configurazione di protezione dell'host non sia cambiata.

- **Miglioramenti della risoluzione dei problemi**

    Abbiamo anche semplificato la [risoluzione dei problemi delle macchine virtuali schermate](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms) abilitando il supporto per la modalità sessione avanzata VMConnect e PowerShell Direct. Questi strumenti sono particolarmente utili se hai perso la connettività di rete alla macchina virtuale e devi aggiornarne la configurazione per ripristinare l'accesso. 

    Queste funzionalità non devono essere configurate e vengono rese disponibili automaticamente quando una macchina virtuale schermata viene posizionata su un host Hyper-V che esegue Windows Server versione 1803 o successiva.

- **Supporto di Linux**

    Se esegui ambienti di sistema operativo misto, adesso Windows Server 2019 supporta l'esecuzione di Ubuntu, Red Hat Enterprise Linux e SUSE Linux Enterprise Server in macchine virtuali schermate.

### HTTP/2 per un Web più veloce e sicuro

- Unione delle connessioni migliorata per un'esperienza di esplorazione senza interruzioni e correttamente crittografata.

- Negoziazione del pacchetto di crittografia lato server di HTTP/2 aggiornata per la prevenzione automatica di errori di connessione e facilità di distribuzione.

- Il provider di congestione TCP predefinito è stato modificato in Cubico per offrirti una velocità effettiva maggiore.

## Archiviazione

Ecco alcune delle modifiche che abbiamo apportato all'archiviazione in Windows Server 2019. Per altre informazioni, vedi [Novità in Archiviazione](../storage/whats-new-in-storage.md).

### Servizio di migrazione della risorsa di archiviazione

Il servizio di migrazione della risorsa di archiviazione è una nuova tecnologia che semplifica la migrazione dei server a una versione più recente di Windows Server. Fornisce uno strumento grafico che esegue l'inventario dei dati nei server, trasferisce i dati e la configurazione nei server più recenti e, facoltativamente, sposta le identità dei server precedenti ai nuovi server in modo che le app e gli utenti non debbano apportare alcuna modifica. Per altre info, vedi [Servizio di migrazione della risorsa di archiviazione](../storage/storage-migration-service/overview.md).

### Spazi di archiviazione diretta

Ecco un elenco delle novità in Spazi di archiviazione diretta. Per altre informazioni, vedi [Novità in Spazi di archiviazione diretta](../storage/whats-new-in-storage.md#storage-spaces-direct).

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

### Replica di archiviazione

Ecco le novità in Replica di archiviazione. Per altre informazioni, vedi [Novità in Replica di archiviazione](../storage/whats-new-in-storage.md#storage-replica).

- Replica di archiviazione è ora disponibile in Windows Server 2019 Standard Edition.
- Il failover di test è una nuova funzionalità che consente il montaggio dell'archiviazione di destinazione per convalidare i dati di replica o di backup. Per altre informazioni, vedi [Domande frequenti su Replica di archiviazione](../storage/storage-replica/storage-replica-frequently-asked-questions.md).
- Miglioramenti delle prestazioni del log per Replica di archiviazione
- Supporto di Windows Admin Center

## Clustering di failover

Ecco un elenco delle novità in Clustering di failover. Per altre informazioni, vedi [Novità in Clustering di failover](../failover-clustering/whats-new-in-failover-clustering.md).

- **Set di cluster**
- **Cluster compatibili con Azure**
- **Migrazione di cluster tra domini**
- **Controllo USB**
- **Miglioramenti all'infrastruttura cluster**
- **L'aggiornamento compatibile con i cluster supporta Spazi di archiviazione diretta**
- **Miglioramenti al controllo della condivisione file**
- **Protezione avanzata del cluster**
- **Il cluster di failover non usa più l'autenticazione NTLM**

## Piattaforma per applicazioni

### Contenitori di Linux in Windows

Ora è possibile eseguire contenitori basati su Linux e Windows nello stesso host contenitore, utilizzando lo stesso daemon docker. Ciò ti consente di disporre di un ambiente costituito da host contenitori eterogenei, offrendo nel contempo flessibilità agli sviluppatori di applicazioni.

### Realizzazione del supporto per Kubernetes

Windows Server 2019 apporta continui miglioramenti in fatto di elaborazione, rete e archiviazione attraverso i rilasci del Canale semestrale, necessari per supportare Kubernetes su Windows. Altri dettagli sono disponibili nelle versioni future di Kubernetes.

- [Rete di contenitori](https://docs.microsoft.com/windows-server/networking/sdn/technologies/containers/container-networking-overview) in Windows Server 2019 migliora notevolmente l'usabilità di Kubernetes in Windows migliorando la resilienza di rete della piattaforma e il supporto di plug-in della rete dei contenitori.

- I carichi di lavoro distribuiti su Kubernetes sono in grado di usare la protezione di rete per la protezione dei servizi Linux e Windows con strumenti incorporati.

### Miglioramenti dei contenitori
    
- **Identità integrata avanzata**

    L'impostazione Autenticazione integrata di Windows nei contenitori è stata resa più semplice e affidabile, risolvendo così diverse limitazioni rispetto alle versioni precedenti di Windows Server.

- **Migliore compatibilità delle applicazioni**

    L'inserimento di applicazioni basate su Windows in un contenitore è stato semplificato: la compatibilità delle app per l'immagine *windowsservercore* esistente è stata migliorata. Per le applicazioni con dipendenze API aggiuntive, è ora disponibile una terza immagine di base: *windows*.

- **Dimensioni ridotte e prestazioni superiori**

    Le dimensioni di download dell'immagine contenitore di base, la dimensione su disco e i tempi di avvio sono stati migliorati. Questo accelera i flussi di lavoro del contenitore

- **Esperienza di gestione con Windows Admin Center \(preview\)**

    Vedere quali contenitori sono in esecuzione nel computer e gestire i singoli contenitori con una nuova estensione per Windows Admin Center non è mai stato così semplice. Cerca l'estensione "Containers" nel [feed pubblico di Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/using-extensions).

### Reti crittografate

[Reti codificate](https://docs.microsoft.com/windows-server/networking/sdn/sdn-whats-new) - La codifica di rete virtuale consente la codifica del traffico di rete virtuale tra le macchine virtuali che comunicano tra loro all'interno di subnet contrassegnate come **Codifica abilitata**. Utilizza anche Datagram Transport Layer Security (DTLS) nella subnet virtuale per codificare pacchetti. DTLS impedisce intercettazioni, manomissioni e contraffazioni da parte di chiunque abbia accesso alla rete fisica.

### Miglioramenti nelle prestazioni di rete per i carichi di lavoro virtuali

[Miglioramenti alle prestazioni di rete per i carichi di lavoro virtuali](https://docs.microsoft.com/windows-server/networking/technologies/hpn/hpn-insider-preview) consente di ottimizzare la velocità di rete sulle macchine virtuali senza che dover costantemente sincronizzare o effettuare il provisioning eccessivo dell'host. Questo riduce le operazioni e i costi di manutenzione, migliorando la densità disponibile degli host. Queste nuove funzionalità sono:

* Unione segmenti ricevuti (RSC) nel commutatore virtuale

* Coda di più macchine virtuali dinamica (d.VMMQ)

### Low Extra Delay Background Transport

Low Extra Delay Background Transport (LEDBAT) è un provider di controllo della congestione della rete, ottimizzato per la latenza, progettato per produrre automaticamente larghezza di banda per gli utenti e le applicazioni, che usa l'intera larghezza di banda disponibile quando la rete non è in uso.   
Questa tecnologia è progettata per l'uso nella distribuzione di aggiornamenti critici e di grandi dimensioni in un ambiente IT senza influire sui servizi per i clienti e sulla larghezza di banda associata.

### Servizio Ora di Windows

Il [servizio Ora di Windows](https://docs.microsoft.com/windows-server/networking/windows-time-service/insider-preview) include un reale supporto di secondi intercalari conforme a UTC, un nuovo protocollo orario chiamato Precision Time Protocol e tracciabilità end-to-end.


### Gateway SDN ad alte prestazioni

[I gateway SDN ad alte prestazioni](https://docs.microsoft.com/windows-server/networking/sdn/gateway-performance) in Windows Server 2019 migliorano notevolmente le prestazioni per le connessioni IPsec e GRE, fornendo una velocità effettiva molto elevata con un utilizzo della CPU molto inferiore.
<br/>

### Nuova estensione dell'interfaccia di distribuzione e di Windows Admin Center per SDN

Con Windows Server 2019, è facile distribuire e gestire tramite una nuova interfaccia utente di sviluppo e un'estensione di Windows Admin Center che consentono a tutti di sfruttare le potenzialità di SDN. 

### Supporto della memoria persistente per macchine virtuali Hyper-V

Per sfruttare la velocità effettiva elevata e la bassa latenza della memoria persistente (ovvero memoria della classe di archiviazione) nelle macchine virtuali, adesso può essere proiettata direttamente nelle macchine virtuali. Ciò consente di ridurre notevolmente la latenza di transazione del database o i tempi di ripristino per i database in memoria a bassa latenza in caso di errore.

