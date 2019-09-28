---
title: Domande frequenti sul servizio migrazione archiviazione
description: Domande frequenti sul servizio migrazione archiviazione, ad esempio i file esclusi dai trasferimenti quando si esegue la migrazione da un server a un altro.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 08/19/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.openlocfilehash: a776920caa85c3ee133070d52b020c8ad3c799e1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403000"
---
# <a name="storage-migration-service-frequently-asked-questions-faq"></a>Domande frequenti sul servizio migrazione archiviazione

Questo argomento contiene le risposte alle domande frequenti sull'uso del [servizio migrazione archiviazione](overview.md) per eseguire la migrazione dei server.

## <a name="what-files-and-folders-are-excluded-from-transfers"></a>Quali file e cartelle sono esclusi dai trasferimenti?

Il servizio migrazione archiviazione non trasferirà i file o le cartelle che sappiamo potrebbero interferire con l'operazione di Windows. In particolare, ecco gli elementi che non verranno trasferiti o spostati nella cartella PreExistingData nella destinazione:

- Windows, programmi, file di programma (x86), dati del programma, utenti
- $Recycle. bin, cestino, riciclato, informazioni sul volume di sistema, $UpgDrv $, $SysReset, $Windows. ~ BT, $Windows. ~ LS, Windows. old, boot, Recovery, Documents and Settings
- Pagefile. sys, hiberfil. sys, file. sys, winpepge. sys, config. sys, Bootsect. bak, BOOTMGR, bootnxt
- Tutti i file o le cartelle del server di origine che sono in conflitto con le cartelle escluse nella destinazione. <br>Ad esempio, se è presente una cartella N:\Windows nell'origine e ne viene eseguito il mapping a C:\ il volume nella destinazione non viene trasferito, indipendentemente dal contenuto, perché interferisce con la cartella di sistema C:\Windows nella destinazione.

## <a name="are-domain-migrations-supported"></a>Le migrazioni di dominio sono supportate?

Il servizio migrazione archiviazione non consente la migrazione tra domini Active Directory. Le migrazioni tra i server aggiungeranno sempre il server di destinazione allo stesso dominio. È possibile usare le credenziali di migrazione da domini diversi nella foresta Active Directory. Il servizio migrazione archiviazione supporta la migrazione tra gruppi di lavoro.  

## <a name="are-clusters-supported-as-sources-or-destinations"></a>I cluster sono supportati come origini o destinazioni?

Il servizio migrazione archiviazione attualmente non esegue la migrazione tra cluster in Windows Server 2019. Si prevede di aggiungere il supporto per i cluster in una versione futura del servizio migrazione archiviazione.

## <a name="do-local-groups-and-local-users-migrate"></a>Migrare i gruppi locali e gli utenti locali?

Il servizio migrazione archiviazione attualmente non esegue la migrazione di utenti locali o gruppi locali in Windows Server 2019. Si prevede di aggiungere il supporto per la migrazione di utenti e gruppi locali in una versione futura del servizio migrazione archiviazione.

## <a name="is-domain-controller-migration-supported"></a>La migrazione del controller di dominio è supportata?

Il servizio migrazione archiviazione attualmente non esegue la migrazione dei controller di dominio in Windows Server 2019. Come soluzione alternativa, se si dispone di più di un controller di dominio nel dominio Active Directory, abbassare di livello il controller di dominio prima di eseguirne la migrazione, quindi alzare di livello la destinazione al termine del trasferimento. Si prevede di aggiungere il supporto per la migrazione del controller di dominio in una versione futura del servizio migrazione archiviazione.

## <a name="what-attributes-are-migrated-by-the-storage-migration-service"></a>Quali attributi vengono migrati dal servizio migrazione archiviazione?

Il servizio migrazione archiviazione esegue la migrazione di tutti i flag, le impostazioni e la protezione delle condivisioni SMB. L'elenco dei flag migrati dal servizio migrazione archiviazione include:

    - Stato condivisione
    - Tipo di disponibilità
    - Tipo di condivisione
    - Modalità di enumerazione delle cartelle *(noto come enumerazione basata sull'accesso o ABE)*
    - Modalità di memorizzazione nella cache
    - Modalità di leasing
    - Istanza SMB
    - Timeout CA
    - Limite utenti simultanei
    - Disponibile in modo continuo
    - Descrizione           
    - Crittografa dati
    - Comunicazione remota delle identità
    - Infrastruttura
    - Nome
    - `Path`
    - Ambito
    - Nome dell'ambito
    - Descrittore di sicurezza
    - Copia shadow
    - Speciali
    - Temporanea

## <a name="can-i-consolidate-multiple-servers-into-one-server"></a>È possibile consolidare più server in un unico server?

La versione del servizio migrazione archiviazione fornita in Windows Server 2019 non supporta il consolidamento di più server in un unico server. Un esempio di consolidamento consiste nell'eseguire la migrazione di tre server di origine distinti, che possono avere gli stessi nomi di condivisione e percorsi di file locali, in un singolo server nuovo che ha virtualizzato i percorsi e le condivisioni per evitare sovrapposizioni o conflitti, quindi ha risposto a tutti e tre nomi di server e indirizzi IP precedenti. Questa funzionalità può essere aggiunta in una versione futura del servizio migrazione archiviazione. 

## <a name="can-i-migrate-from-sources-other-than-windows-server"></a>È possibile eseguire la migrazione da origini diverse da Windows Server?

La versione del servizio migrazione archiviazione fornita in Windows Server 2019 supporta la migrazione da sistemi operativi Windows Server 2003 e versioni successive. È anche possibile eseguire la migrazione dello spazio di archiviazione da un server Linux o da un dispositivo che usa Samba; a tale scopo, eseguire il servizio migrazione archiviazione in un server che esegue Windows Server, versione 1903 o successiva.

## <a name="can-i-migrate-previous-file-versions"></a>È possibile eseguire la migrazione delle versioni precedenti del file?

La versione del servizio di migrazione archiviazione fornita in Windows Server 2019 non supporta la migrazione di versioni precedenti (effettuate con il servizio Copia Shadow del volume) dei file. Viene eseguita la migrazione solo della versione corrente. 

## <a name="optimizing-inventory-and-transfer-performance"></a>Ottimizzazione delle prestazioni di inventario e trasferimento

Il servizio di migrazione archiviazione contiene un motore di lettura e copia multithread denominato servizio proxy del servizio di migrazione archiviazione progettato per essere veloce, oltre a fornire una perfetta fedeltà dei dati priva di molti strumenti per la copia di file. Sebbene la configurazione predefinita sia ottimale per molti clienti, esistono modi per migliorare le prestazioni degli SMS durante l'inventario e il trasferimento.

- **Usare Windows Server 2019 per il sistema operativo di destinazione.** Windows Server 2019 contiene il servizio proxy del servizio migrazione archiviazione. Quando si installa questa funzionalità e si esegue la migrazione a destinazioni di Windows Server 2019, tutti i trasferimenti funzionano come linea di visualizzazione diretta tra l'origine e la destinazione. Questo servizio viene eseguito nell'agente di orchestrazione durante il trasferimento se i computer di destinazione sono Windows Server 2012 R2 o Windows Server 2016, il che significa che i trasferimenti a doppio hop e saranno molto più lenti. Se sono in esecuzione più processi con le destinazioni di Windows Server 2012 R2 o Windows Server 2016, l'agente di orchestrazione diventa un collo di bottiglia. 

- **Modifica i thread di trasferimento predefiniti.** Il servizio proxy del servizio migrazione archiviazione copia 8 file contemporaneamente in un determinato processo. È possibile aumentare il numero di thread di copia simultanei modificando il nome del valore REG_DWORD del registro di sistema seguente in Decimal in ogni nodo in cui è in esecuzione il proxy del servizio migrazione archiviazione:

    HKEY_Local_Machine\Software\Microsoft\SMSProxy
    
    FileTransferThreadCount

   L'intervallo valido è compreso tra 1 e 128 in Windows Server 2019. Dopo la modifica è necessario riavviare il servizio proxy del servizio migrazione archiviazione in tutti i computer che partecipano a una migrazione. Prestare attenzione con questa impostazione. l'impostazione di un valore superiore potrebbe richiedere Core aggiuntivi, prestazioni di archiviazione e larghezza di banda di rete. L'impostazione di un valore troppo alto può comportare una riduzione delle prestazioni rispetto alle impostazioni predefinite.

- **Aggiungere core e memoria.**  Si consiglia vivamente che l'origine, l'agente di orchestrazione e i computer di destinazione dispongano di almeno due core del processore o due vCPU e più possono aiutare significativamente le prestazioni di inventario e trasferimento, soprattutto se combinate con FileTransferThreadCount (sopra). Quando si trasferiscono file di dimensioni superiori ai normali formati di Office (gigabyte o superiori), le prestazioni di trasferimento trarranno vantaggio da una quantità di memoria superiore a quella del valore minimo predefinito di 2 GB.

- **Creare più processi.** Quando si crea un processo con più origini server, ogni server viene contattato in modo seriale per l'inventario, il trasferimento e la cutover. Ciò significa che ogni server deve completare la propria fase prima dell'avvio di un altro server. Per eseguire più server in parallelo, è sufficiente creare più processi, ognuno dei quali contiene un solo server. SMS supporta fino a 100 processi in esecuzione simultanea, ovvero un singolo agente di orchestrazione può parallelizzare molti computer di destinazione Windows Server 2019. Non è consigliabile eseguire più processi paralleli se i computer di destinazione sono Windows Server 2016 o Windows Server 2012 R2 come senza il servizio proxy SMS in esecuzione nella destinazione, l'agente di orchestrazione deve eseguire tutti i trasferimenti e potrebbe diventare collo. La possibilità per i server di essere eseguiti in parallelo all'interno di un singolo processo è una funzionalità che si prevede di aggiungere in una versione successiva di SMS.

- **Usare SMB 3 con reti RDMA.** Se si trasferisce da un computer di origine Windows Server 2012 o versione successiva, SMB 3. x supporta la modalità SMB diretto e la rete RDMA. RDMA sposta la maggior parte del costo della CPU del trasferimento dalle CPU della scheda madre al caricamento dei processori NIC, riducendo la latenza e l'utilizzo della CPU del server. Inoltre, le reti RDMA, ad esempio ROCE e iWARP, dispongono in genere di una larghezza di banda notevolmente superiore rispetto alla tipica TCP/Ethernet, incluse le velocità di 25, 50 e 100 GB per interfaccia. L'utilizzo di SMB diretto in genere sposta il limite di velocità di trasferimento dalla rete allo spazio di archiviazione.   

- **Usare SMB 3 multicanale.** Se si trasferisce da un computer di origine Windows Server 2012 o versione successiva, SMB 3. x supporta le copie multicanale che consentono di migliorare significativamente le prestazioni di copia dei file. Questa funzionalità funziona automaticamente a condizione che l'origine e la destinazione siano entrambe:

   - Più schede di rete
   - Una o più schede di rete che supportano Receive-Side Scaling (RSS)
   - Una o più schede di rete configurate tramite gruppo NIC
   - Una o più schede di rete che supportano RDMA

- **Aggiornare i driver.** Installare i driver più recenti per l'archiviazione e l'enclosure del fornitore, i driver HBA più recenti, il BIOS o il firmware UEFI più recente, i driver di rete del fornitore più recenti e i driver più recenti del chipset della scheda madre sull'origine, sulla destinazione e sull'agente di orchestrazione Server. Se necessario riavviare i nodi. Per la configurazione dell'archiviazione condivisa e dell'hardware di rete, consultare la documentazione del fornitore hardware.

- **Abilitare l'elaborazione ad alte prestazioni.** Assicurarsi che le impostazioni BIOS/UEFI dei server abilitati consentano alte prestazioni, ad esempio la disabilitazione dei C-State, l'impostazione della velocità QPI, l'abilitazione di NUMA e l'impostazione della frequenza di memoria massima. Verificare che il risparmio energia in Windows Server sia impostato su prestazioni elevate. Riavviare se necessario. Non dimenticare di restituirli agli stati appropriati dopo il completamento della migrazione. 

- **Ottimizzazione dell'hardware** Esaminare le [linee guida per l'ottimizzazione delle prestazioni per Windows server 2016](https://docs.microsoft.com/windows-server/administration/performance-tuning/) per l'ottimizzazione dell'agente di orchestrazione e dei computer di destinazione che eseguono windows server 2019 e windows server 2016. La sezione [ottimizzazione delle prestazioni del sottosistema di rete](https://docs.microsoft.com/windows-server/networking/technologies/network-subsystem/net-sub-performance-tuning-nics) contiene informazioni particolarmente utili.

- **Usare una risorsa di archiviazione più veloce.** Sebbene possa essere difficile aggiornare la velocità di archiviazione del computer di origine, è necessario assicurarsi che l'archiviazione di destinazione sia almeno veloce alle prestazioni di i/o di scrittura, perché l'origine è a prestazioni di i/o di lettura per assicurarsi che non sia presente un collo di bottiglia non necessario nei trasferimenti. Se la destinazione è una macchina virtuale, assicurarsi che, almeno ai fini della migrazione, venga eseguita nel livello di archiviazione più veloce degli host hypervisor, ad esempio nel livello Flash o con Spazi di archiviazione diretta cluster HCI che usano gli spazi tutti i flash o ibridi con mirroring. Quando la migrazione di SMS è completata, è possibile eseguire la migrazione in tempo reale della macchina virtuale a un livello più lento o a un host.

- **Aggiornare l'antivirus.** Assicurarsi sempre che l'origine e la destinazione eseguano la versione più recente con patch del software antivirus per garantire un overhead minimo delle prestazioni. Come test, è possibile escludere *temporaneamente* l'analisi delle cartelle di cui è in corso l'inventario o la migrazione nei server di origine e di destinazione. Se le prestazioni di trasferimento risultano migliorate, contattare il fornitore del software antivirus per istruzioni o per una versione aggiornata del software antivirus o una spiegazione della riduzione delle prestazioni prevista.

## <a name="can-i-migrate-from-ntfs-to-refs"></a>È possibile eseguire la migrazione da NTFS a REFS?

La versione del servizio di migrazione archiviazione fornita in Windows Server 2019 non supporta la migrazione dal file system NTFS ai file REFS. È possibile eseguire la migrazione da NTFS a NTFS e REFS a ReFS. Questo è dovuto alla progettazione, a causa delle numerose differenze nella funzionalità, nei metadati e in altri aspetti che ReFS non duplica da NTFS. ReFS è destinato a un carico di lavoro dell'applicazione file system, non a una file system generale. Per ulteriori informazioni, vedere [Panoramica di Resilient file System (Refs)](../refs/refs-overview.md) . 

## <a name="can-i-move-the-storage-migration-service-database"></a>È possibile spostare il database del servizio migrazione archiviazione?

Il servizio migrazione archiviazione usa un database Extensible Storage Engine (ESE) che viene installato per impostazione predefinita nella cartella Hidden c:\programdata\microsoft\storagemigrationservice Questo database aumenterà man mano che i processi vengono aggiunti e i trasferimenti vengono completati e possono utilizzare spazio su disco significativo dopo la migrazione di milioni di file se non si eliminano i processi. Se è necessario spostare il database, seguire questa procedura:

1. Arrestare il servizio "servizio migrazione archiviazione" sul computer dell'agente di orchestrazione.
2. Assumere la `%programdata%/Microsoft/StorageMigrationService` proprietà della cartella
3. Aggiungere l'account utente per avere il controllo completo su tale condivisione e su tutti i relativi file e sottocartelle.
4. Spostare la cartella in un'altra unità sul computer dell'agente di orchestrazione.
5. Impostare il seguente valore REG_SZ del registro di sistema:

    HKEY_Local_Machine\Software\Microsoft\SMS DatabasePath = *percorso alla nuova cartella del database in un volume diverso* . 
6. Verificare che il sistema disponga del controllo completo su tutti i file e le sottocartelle della cartella
7. Rimuovere le autorizzazioni per gli account personali.
8. Avviare il servizio "servizio migrazione archiviazione".

## <a name="give-feedback"></a>Quali sono le opzioni disponibili per inviare commenti e suggerimenti, archiviare bug o ottenere supporto?

Per inviare commenti e suggerimenti sul servizio migrazione archiviazione:

- Usare lo strumento Hub di feedback incluso in Windows 10, facendo clic su "Suggerisci una funzionalità" e specificando la categoria "Windows Server" e la sottocategoria "migrazione archiviazione".
- Usare il sito [Windows Server UserVoice](https://windowsserver.uservoice.com)
- Posta elettronicasmsfeed@microsoft.com

Per archiviare i bug:

- Usare lo strumento Hub di feedback incluso in Windows 10, facendo clic su "segnala un problema" e specificando la categoria "Windows Server" e la sottocategoria "migrazione archiviazione".
- Apri un caso di supporto tramite [supporto tecnico Microsoft](https://support.microsoft.com)

Per ottenere supporto:

 - Pubblica una domanda nella [community di tecnologia Windows Server](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)
 - Pubblicare un post nel [Forum di TechNet su Windows Server 2019](https://social.technet.microsoft.com/Forums/en-US/home?forum=ws2019&filter=alltypes&sort=lastpostdesc) 
 - Apri un caso di supporto tramite [supporto tecnico Microsoft](https://support.microsoft.com)

## <a name="see-also"></a>Vedere anche

- [Panoramica di servizio migrazione archiviazione](overview.md)
