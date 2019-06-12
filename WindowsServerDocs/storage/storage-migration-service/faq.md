---
title: Domande frequenti (FAQ) il servizio di migrazione archiviazione
description: Domande frequenti sul servizio di migrazione di archiviazione, ad esempio i file che sono esclusi dal trasferimento durante la migrazione da un server a un altro.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 06/04/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 258f25a7e1ec5c796c15450625397e96db25d693
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501517"
---
# <a name="storage-migration-service-frequently-asked-questions-faq"></a>Domande frequenti (FAQ) il servizio di migrazione archiviazione

In questo argomento è fornite le risposte alle domande frequenti (FAQ) sull'uso [servizio di migrazione archiviazione](overview.md) la migrazione di server.

## <a name="excluded-files"></a> Quali file e cartelle vengono esclusi dal trasferimento?

Il servizio di migrazione di archiviazione non trasferire file o cartelle che sappiamo che potrebbero interferire con l'operazione di Windows. In particolare, ecco cosa abbiamo non trasferire o spostare nella cartella PreExistingData nella destinazione:

- Windows, file, i file di programma (x86), i dati del programma, gli utenti di programma
- $Recycle.bin, recycler, Recycled, System Volume Information, $UpgDrv$, $SysReset, $Windows. ~ BT, $Windows. ~ LS, Windows. old, avvio, ripristino, documenti e impostazioni
- pagefile.sys, hiberfil.sys, swapfile.sys, winpepge.sys, config.sys, bootsect.bak, bootmgr, bootnxt
- Qualsiasi file o cartelle nel server di origine che è in conflitto con le cartelle escluse nella destinazione. <br>Ad esempio, se è presente una cartella N:\Windows nell'origine e viene eseguito il mapping da C:\ volume nella destinazione, non venga trasferito, indipendentemente dal relativo contenuto, perché potrebbe interferire con la cartella di sistema C:\Windows nella destinazione.

## <a name="domain-migration"></a> Sono supportate le migrazioni di dominio?

Il servizio di migrazione di archiviazione non consente di eseguire la migrazione tra domini di Active Directory. Le migrazioni tra server sempre verranno aggiunto il server di destinazione allo stesso dominio. È possibile usare le credenziali di migrazione da altri domini nella foresta Active Directory. Il servizio di migrazione di archiviazione supporta la migrazione tra gruppi di lavoro.  

## <a name="cluster-support"></a> I cluster sono supportati come origini o destinazioni?

Il servizio di migrazione di archiviazione attualmente non eseguire la migrazione tra cluster in Windows Server 2019. Si prevede di aggiungere il supporto di cluster in una versione futura del servizio di migrazione di archiviazione.

## <a name="local-principals"></a> Gruppi locali e locale agli utenti di eseguire la migrazione?

Il servizio di migrazione di archiviazione attualmente non eseguire la migrazione gli utenti locali o i gruppi locali in Windows Server 2019. Si prevede di aggiungere supporto utente locale e migrazione dei gruppi locali in una versione futura del servizio di migrazione di archiviazione.

## <a name="domain-controller"></a> È supportata la migrazione del controller di dominio?

Il servizio di migrazione di archiviazione attualmente non eseguire la migrazione di controller di dominio in Windows Server 2019. In alternativa, purché si dispone di più di un controller di dominio nel dominio di Active Directory, abbassare di livello il controller di dominio prima di eseguire la migrazione, quindi alzare di livello la destinazione dopo il completamento di trasferimento. Si prevede di aggiungere il supporto di migrazione del controller di dominio in una versione futura del servizio di migrazione di archiviazione.

## <a name="share-attributes"></a> Quali attributi vengono migrati dal servizio di migrazione di archiviazione?

Il servizio di migrazione di archiviazione esegue la migrazione di tutti i flag, le impostazioni e sicurezza delle condivisioni SMB. Tale elenco di flag che viene eseguita la migrazione di servizio di migrazione di archiviazione include:

    - Stato di condivisione
    - Tipo di disponibilità
    - Tipo di condivisione
    - Modalità di enumerazione cartella *(noto anche come basata sull'accesso enumerazione o ABE)*
    - Modalità di memorizzazione nella cache
    - Leasing di modalità
    - Istanza di SMB
    - Timeout di autorità di certificazione
    - Limite di utenti simultanei
    - Sempre disponibile
    - Descrizione           
    - Crittografare i dati
    - Comunicazione remota di identità
    - Infrastruttura
    - Nome
    - `Path`
    - Con ambito
    - Nome dell'ambito
    - Descrittore di sicurezza
    - Copia shadow
    - Speciale
    - Temporaneo

## <a name="move-db"></a> È possibile spostare il database del servizio di migrazione di archiviazione?

Il servizio di migrazione di archiviazione Usa un database di extensible storage engine (ESE) viene installato per impostazione predefinita nella cartella c:\programdata\microsoft\storagemigrationservice nascosti. Questo database aumenterà man mano che vengono aggiunti i processi e i trasferimenti vengono completati e possono usare spazio su disco significativo dopo la migrazione di milioni di file se non si elimina i processi. Se il database deve essere spostato, procedere come segue:

1. Arrestare il servizio "Servizio di migrazione di archiviazione" sul computer dell'agente di orchestrazione.
2. Acquisire la proprietà del `%programdata%/Microsoft/StorageMigrationService` cartella
3. Aggiungere l'account utente hanno pieno controllo sulle che condividono e tutti i relativi file e sottocartelle.
4. Spostare la cartella in un'altra unità nel computer dell'agente di orchestrazione.
5. Impostare il Registro di sistema valore REG_SZ seguente:

    DatabasePath HKEY_Local_Machine\Software\Microsoft\SMS = *percorso della cartella di database nuovo in un volume diverso* . 
6. Assicurarsi che il sistema abbia il controllo completo a tutti i file e sottocartelle della cartella
7. Rimuovere le autorizzazioni il proprio account.
8. Avviare il servizio "Servizio di migrazione di archiviazione".

## <a name="non-windows"></a> È possibile migrare da origini diverse da Windows Server?

La versione del servizio di migrazione di archiviazione fornita in Windows Server 2019 supporta la migrazione da Windows Server 2003 e sistemi operativi successivi. Attualmente non è possibile eseguire la migrazione da Linux, Samba, NetApp, EMC o altri dispositivi di archiviazione SAN e NAS. Si prevede di consentire questa operazione in una versione futura la migrazione del servizio di archiviazione, a partire con il supporto Linux Samba.

## <a name="previous-versions"></a> È possibile eseguire la migrazione versioni precedenti dei file?

La versione del servizio di migrazione di archiviazione fornita in Windows Server 2019 non supporta versioni precedenti la migrazione (eseguito con il servizio Copia shadow del volume) dei file. Verrà eseguita la migrazione solo la versione corrente. 

## <a name="ntfs-refs"></a> È possibile migrare da NTFS a REFS?

La versione del servizio di migrazione di archiviazione fornita in Windows Server 2019 non supporta la migrazione da NTFS per i file System (Refs). È possibile eseguire la migrazione da NTFS per NTFS e REFS in ReFS. Si tratta per impostazione predefinita, a causa di molte delle differenze nelle funzionalità, i metadati e altri aspetti che ReFS non duplichi da NTFS. ReFS è da intendersi come un sistema di file del carico di lavoro dell'applicazione, non è un sistema di file generici. Per altre informazioni, vedere [Panoramica Resilient File System (ReFS)](../refs/refs-overview.md)

## <a name="consolidate-servers"></a> È possibile consolidare più server in un singolo server?

La versione del servizio di migrazione di archiviazione fornita in Windows Server 2019 non supporta il consolidamento di più server in un unico server. Un esempio di consolidamento consiste nella migrazione tre server di origine separato - che può avere gli stessi nomi di condivisione e percorsi di file locali, in un unico server nuovo virtualizzato questi percorsi e le condivisioni per evitare eventuali collisioni, o si sovrappongono quindi ha risposto a tutte e tre i nomi dei server precedente e l'indirizzo IP. È possibile aggiungere questa funzionalità in una versione futura del servizio di migrazione di archiviazione.  

## <a name="optimize"></a> Ottimizzazione delle prestazioni di inventario e il trasferimento

Il servizio di migrazione di archiviazione contiene un motore di copia, denominato servizio di Proxy del servizio migrazione archiviazione che è progettato per essere sia veloce, nonché di trasferimento di dati ideale fedeltà manca di molti strumenti di copia di file e lettura multithread. Durante la configurazione predefinita sarà ottimale per molti clienti, esistono modi per migliorare le prestazioni di SMS durante l'inventario e il trasferimento.

- **Usare Windows Server 2019 per il sistema operativo di destinazione.** Windows Server 2019 contiene il servizio Proxy del servizio migrazione archiviazione. Quando si installa la funzionalità e la migrazione a Windows Server 2019 destinazioni, tutti i trasferimenti di operano come linea di visuale diretta tra origine e destinazione. Questo servizio viene eseguito sull'agente di orchestrazione durante il trasferimento se il computer di destinazione sono Windows Server 2012 R2 o Windows Server 2016, ovvero i trasferimenti double-hop e sarà decisamente più lento. Se sono presenti più processi in esecuzione con le destinazioni di Windows Server 2016 o Windows Server 2012 R2, l'agente di orchestrazione diventa un collo di bottiglia. 

- **Modificare i thread di trasferimento predefinito.** Il servizio Proxy di servizio di migrazione di archiviazione copia 8 file contemporaneamente in un processo specifico. È possibile aumentare il numero di thread di copia simultanee modificando il nome del valore REG_DWORD del Registro di sistema seguente in formato decimale in ogni nodo che esegue il Proxy di SMS:

    HKEY_Local_Machine\Software\Microsoft\SMSProxy   FileTransferThreadCount

   L'intervallo valido è 1 e 128 in Windows Server 2019. Dopo la modifica è necessario riavviare il servizio Proxy di servizio di migrazione di archiviazione in tutti i computer che fanno parte di una migrazione. Prestare attenzione con questa impostazione; impostarlo superiore richiedano core aggiuntivi, le prestazioni di archiviazione e larghezza di banda di rete. Impostarlo troppo elevato potrebbe causare una riduzione delle prestazioni rispetto alle impostazioni predefinite. La possibilità di modificare in modo euristico basate su CPU, memoria, rete e archiviazione delle impostazioni di thread è pianificata per una versione successiva di SMS.

- **Aggiungere memoria e Core.**  È consigliabile che i computer di origine, orchestrator e di destinazione dispongono di almeno due core di processori o due Vcpu e più agevolano notevolmente le prestazioni di inventario e il trasferimento, soprattutto se combinate con FileTransferThreadCount (sopra). Durante il trasferimento di file più grandi rispetto ai formati Office consueto (gigabyte o versione successiva) delle prestazioni di trasferimento trarranno vantaggio dalla maggiore quantità di memoria rispetto al minimo di 2GB predefinita.

- **Creare più processi.** Quando si crea un processo con più origini di server, in modo seriale per l'inventario, il trasferimento, viene contattato ogni server e della migrazione completa. Ciò significa che ogni server deve completare la fase prima dell'avvio di un altro server. Per eseguire più server in parallelo, è sufficiente creare più processi, in ogni processo contenente un solo server. SMS supporta fino a 100 in esecuzione simultanea di processi, vale a dire che un singolo agente di orchestrazione possono parallelizzare molti computer di destinazione Windows Server 2019. Si consiglia di non in esecuzione più processi paralleli se i computer di destinazione sono Windows Server 2016 o Windows Server 2012 R2 trasferisce se stesso come senza il servizio proxy SMS in esecuzione nella destinazione, l'agente di orchestrazione tutti devono essere eseguiti e potrebbero diventare un collo di bottiglia. La possibilità per i server per l'esecuzione in parallelo all'interno di un singolo processo è una funzionalità che si prevede di aggiungere in una versione successiva di SMS.

- **Usare SMB 3 con le reti RDMA.** Se il trasferimento da Windows Server 2012 o versioni successive computer di origine, SMB 3.x supporta SMB diretto modalità e di rete RDMA. RDMA Sposta la maggior parte dei costi della CPU di trasferimento dalla scheda madre CPU onboard processori di interfaccia di rete, riducendo la latenza e server di utilizzo della CPU. Inoltre, le reti RDMA come ROCE e iWARP hanno in genere notevolmente maggiore larghezza di banda rispetto a TCP tipico/ethernet, tra cui 25, 50 e velocità di 100Gb per ogni interfaccia. In genere utilizzano SMB diretto si sposta il limite di velocità di trasferimento dalla rete fino alla risorsa di archiviazione stesso.   

- **Usare multicanale SMB 3.** Se il trasferimento da un Windows Server 2012 o versioni successive computer di origine, SMB 3.x supporta multicanale le copie che possono migliorare notevolmente le prestazioni di copia del file. Questa funzionalità funziona automaticamente, purché l'origine e destinazione sono:

   - Più schede di rete
   - Uno o più schede di rete che supportano Receive-Side Scaling (RSS)
   - Una delle altre schede di rete vengono configurate tramite gruppo NIC
   - Una o più schede di rete che supportano RDMA

- **Aggiornare i driver.** Se necessario, installare archiviazione fornitore più recenti e il firmware dell'enclosure e driver, i driver HBA del fornitore più recenti, più recente del firmware BIOS o UEFI fornitore, driver di rete fornitore più recenti e driver dei chipset della scheda madre più recenti in origine, destinazione e dell'agente di orchestrazione Server. Se necessario riavviare i nodi. Per la configurazione dell'archiviazione condivisa e dell'hardware di rete, consultare la documentazione del fornitore hardware.

- **Abilitare l'elaborazione ad alte prestazioni.** Assicurarsi che le impostazioni BIOS/UEFI dei server abilitati consentano alte prestazioni, ad esempio la disabilitazione dei C-State, l'impostazione della velocità QPI, l'abilitazione di NUMA e l'impostazione della frequenza di memoria massima. Verificare che il risparmio di energia in Windows Server è impostato su prestazioni elevate. Riavviare se necessario. Non dimenticare di restituire questi state appropriate dopo il completamento della migrazione. 

- **Ottimizzare l'hardware** esaminare i [Performance Tuning linee guida per Windows Server 2016](https://docs.microsoft.com/en-us/windows-server/administration/performance-tuning/) per l'ottimizzazione di orchestrator e computer di destinazione che esegue Windows Server 2019 e Windows Server 2016. Il [ottimizzazione delle prestazioni di rete sottosistema](https://docs.microsoft.com/en-us/windows-server/networking/technologies/network-subsystem/net-sub-performance-tuning-nics) sezione contiene informazioni particolarmente utili.

- **Usare l'archiviazione più veloce.** Anche se potrebbe essere difficile aggiornare la velocità di archiviazione di computer di origine, è necessario assicurarsi che la risorsa di archiviazione di destinazione sia almeno come velocità con cui l'origine è in lettura delle prestazioni dei / o per assicurare che non vi è alcun collo di bottiglia non necessario nei trasferimenti con prestazioni dei / o scrittura. Se la destinazione è una macchina virtuale, verificare che, almeno ai fini della migrazione, l'esecuzione nel livello di archiviazione più veloce gli host hypervisor, ad esempio il livello di memoria flash o con i cluster uomo diretta spazi di archiviazione che usano con mirroring all-flash o ibrida spazi. Una volta completata la migrazione di SMS la macchina virtuale può essere in tempo reale la migrazione a un host o un livello più lento.

- **Aggiornare antivirus.** Assicurarsi sempre l'origine e destinazione sono in esecuzione la versione più recente con patch del software antivirus per garantire un overhead delle prestazioni minimo. Come un test, è possibile *temporaneamente* escludere l'analisi delle cartelle si inventari o la migrazione nei server di origine e destinazione. Se le prestazioni di trasferimento sono stata migliorata, contattare il fornitore di software antivirus per le istruzioni o per una versione aggiornata del software antivirus o una spiegazione di riduzione delle prestazioni previsti.

## <a name="give-feedback"></a> Quali sono le opzioni per fornire commenti e suggerimenti, segnalazione di bug, o ottenere supporto?

Per inviare commenti e suggerimenti sul servizio di migrazione di archiviazione:

- Usare lo strumento di Hub di Feedback incluso in Windows 10, fare clic su "Suggerire una funzionalità" e specificando la categoria di "Windows Server" e una sottocategoria di "Migrazione dell'archiviazione"
- Usare la [Windows Server UserVoice](https://windowsserver.uservoice.com) sito
- Email smsfeed@microsoft.com

Per segnalare bug:

- Usare lo strumento di Hub di Feedback incluso in Windows 10, fare clic su "Segnala un problema" e specificando la categoria di "Windows Server" e una sottocategoria di "Migrazione dell'archiviazione"
- Aprire un caso di supporto tramite [il supporto tecnico Microsoft](https://support.microsoft.com)

Per ottenere supporto:

 - Pubblicare una domanda sul [Tech Community su Windows Server](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)
 - Inserire un post nel [Forum Technet di Windows Server 2019](https://social.technet.microsoft.com/Forums/en-US/home?forum=ws2019&filter=alltypes&sort=lastpostdesc) 
 - Aprire un caso di supporto tramite [il supporto tecnico Microsoft](https://support.microsoft.com)


## <a name="see-also"></a>Vedere anche

- [Panoramica del servizio di migrazione archiviazione](overview.md)
