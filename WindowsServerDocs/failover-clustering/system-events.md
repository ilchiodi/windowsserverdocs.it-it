---
title: Eventi del registro di sistema del clustering di failover
description: Elenco di eventi del clustering di failover nel registro di sistema di Windows Server. Questi eventi possono essere usati per risolvere i problemi relativi a un cluster.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 01/14/2020
ms.openlocfilehash: 5c2606b96b42d08cc66da2e19596240c21bf4b88
ms.sourcegitcommit: 10331ff4f74bac50e208ba8ec8a63d10cfa768cc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/15/2020
ms.locfileid: "75956453"
---
# <a name="failover-clustering-system-log-events"></a>Eventi del registro di sistema del clustering di failover

>Si applica a: Windows Server 2019, Windows Server 2016

Questo argomento elenca gli eventi di clustering di failover dal registro di sistema di Windows Server (visualizzabile in Visualizzatore eventi). Tutti questi eventi condividono l'origine evento di **FailoverClustering** e possono essere utili per la risoluzione dei problemi di un cluster.

## <a name="critical-events"></a>Eventi critici

### <a name="event-1000-unexpected_fatal_error"></a>Evento 1000: UNEXPECTED_FATAL_ERROR

Servizio cluster ha rilevato un errore irreversibile imprevisto alla riga %1 del modulo di origine %2. Codice di errore: %3.

### <a name="event-1001-assertion_failure"></a>Evento 1001: ASSERTION_FAILURE

Non è stato Servizio cluster un controllo di validità alla riga %1 del modulo di origine %2. ' %3'

### <a name="event-1006-nm_event_membership_halt"></a>Evento 1006: NM_EVENT_MEMBERSHIP_HALT

Servizio cluster è stato interrotto a causa di una connettività incompleta con altri nodi del cluster.

### <a name="event-1057-dm_database_corrupt_or_missing"></a>Evento 1057: DM_DATABASE_CORRUPT_OR_MISSING

Impossibile caricare il database del cluster. Il file potrebbe essere mancante o danneggiato.
È possibile che venga effettuato un tentativo di correzione automatica.

### <a name="event-1070-nm_event_begin_join_failed"></a>Evento 1070: NM_EVENT_BEGIN_JOIN_FAILED

Il nodo non è riuscito ad aggiungere il cluster di failover ' %1' a causa del codice errore ' %2'.

### <a name="event-1073-cs_event_inconsistency_halt"></a>Evento 1073: CS_EVENT_INCONSISTENCY_HALT

Il Servizio cluster è stato interrotto per evitare un'incoerenza nel cluster di failover. Codice di errore:' %1'.

### <a name="event-1080-cs_diskwrite_failure"></a>Evento 1080: CS_DISKWRITE_FAILURE

Il Servizio cluster non è riuscito ad aggiornare il database del cluster (codice errore ' %1').
Le possibili cause sono uno spazio su disco insufficiente o file system danneggiamento.

### <a name="event-1090-cs_event_reg_operation_failed"></a>Evento 1090: CS_EVENT_REG_OPERATION_FAILED

Impossibile avviare il Servizio cluster. Il tentativo di leggere i dati di configurazione dal registro di sistema di Windows non è riuscito con errore ' %1'. Utilizzare lo snap-in Gestione cluster di failover per verificare che il computer sia membro di un cluster.
Se si intende aggiungere questo computer a un cluster esistente, usare la procedura guidata Aggiungi nodo. In alternativa, se il computer è stato configurato come membro di un cluster, sarà necessario ripristinare i dati di configurazione mancanti necessari per il servizio cluster per identificare che si tratta di un membro di un cluster.
Eseguire un ripristino dello stato del sistema di questo computer per ripristinare i dati di configurazione.

### <a name="event-1092-nm_event_mm_form_failed"></a>Evento 1092: NM_EVENT_MM_FORM_FAILED

Non è stato possibile creare il cluster ' %1' con codice errore ' %2'. Il cluster di failover non sarà disponibile.

### <a name="event-1093-nm_event_node_not_member"></a>Evento 1093: NM_EVENT_NODE_NOT_MEMBER

Il Servizio cluster non è in grado di identificare il nodo ' %1' come membro del cluster di failover ' %2'. Se il nome del computer per questo nodo è stato modificato di recente, provare a ripristinare il nome precedente. In alternativa, aggiungere il nodo al cluster di failover e, se necessario, reinstallare le applicazioni in cluster.

### <a name="event-1105-cs_event_rpc_init_failed"></a>Evento 1105: CS_EVENT_RPC_INIT_FAILED

Impossibile avviare il Servizio cluster perché non è stato in grado di registrare le interfacce con il servizio RPC. Codice di errore:' %1'.

### <a name="event-1135-event_node_down"></a>Evento 1135: EVENT_NODE_DOWN

Il nodo cluster ' %1' è stato rimosso dall'appartenenza al cluster di failover attivo. È possibile che l'Servizio cluster in questo nodo sia stata arrestata. Questo problema può essere dovuto anche al fatto che il nodo ha perso la comunicazione con altri nodi attivi nel cluster di failover.
Eseguire la convalida guidata configurazione per verificare la configurazione di rete. Se la condizione è permanente, verificare la presenza di errori hardware o software relativi alle schede di rete in questo nodo. Controllare anche la presenza di errori in qualsiasi altro componente di rete a cui è connesso il nodo, ad esempio hub, commutatori o Bridge.

### <a name="event-1146-rcm_event_resmon_died"></a>Evento 1146: RCM_EVENT_RESMON_DIED

Il processo del cluster Resource Hosting Subsystem (RHS) è stato terminato e verrà riavviato. Questa operazione è in genere associata al rilevamento e al ripristino dell'integrità del cluster di una risorsa. Fare riferimento al registro eventi di sistema per determinare la risorsa e la DLL di risorse che causano il problema.

### <a name="event-1177-mm_event_arbitration_failed"></a>Evento 1177: MM_EVENT_ARBITRATION_FAILED

Il Servizio cluster viene arrestato perché il quorum è andato perso. La causa potrebbe essere la perdita della connettività di rete tra alcuni o tutti i nodi del cluster o un failover del disco di controllo.

Eseguire la convalida guidata configurazione per verificare la configurazione di rete. Se la condizione è permanente, verificare la presenza di errori hardware o software correlati alla scheda di rete. Controllare anche la presenza di errori in qualsiasi altro componente di rete a cui è connesso il nodo, ad esempio hub, commutatori o Bridge.

### <a name="event-1181-netres_resource_start_error"></a>Evento 1181: NETRES_RESOURCE_START_ERROR

Impossibile portare in linea la risorsa cluster ' %1' perché il servizio associato non è stato avviato. Il codice restituito del servizio è' %2'. Verificare la presenza di altri eventi associati al servizio e verificare che il servizio venga avviato correttamente.

### <a name="event-1247-cluster_event_invalid_service_sid_type"></a>Evento 1247: CLUSTER_EVENT_INVALID_SERVICE_SID_TYPE

Il tipo di ID di sicurezza (SID) per il servizio cluster è configurato come ' %1', ma il tipo di SID previsto è' Unrestricted '. Il servizio cluster modifica automaticamente la configurazione del tipo di SID con Gestione controllo servizi e verrà riavviato per rendere effettiva la modifica.

### <a name="event-1248-cluster_event_service_sid_missing"></a>Evento 1248: CLUSTER_EVENT_SERVICE_SID_MISSING

L'ID di sicurezza (SID)' %1' associato al servizio cluster non è presente nel token di processo. Il servizio cluster correggerà automaticamente il problema e verrà riavviato.

### <a name="event-1282-sm_event_handshake_timeout"></a>Evento 1282: SM_EVENT_HANDSHAKE_TIMEOUT

L'handshake di sicurezza tra gli endpoint locali e remoti ' %2' non è stato completato in ' %1' secondi, il nodo termina la connessione

### <a name="event-1542-service_prerestore_failed"></a>Evento 1542: SERVICE_PRERESTORE_FAILED

La richiesta di ripristino per i dati di configurazione del cluster non è riuscita. Il ripristino non è riuscito durante la fase di pre-ripristino, in genere indica che alcuni nodi che comprendono il cluster non sono attualmente operativi. Verificare che il servizio cluster sia in esecuzione correttamente in tutti i nodi che includono questo cluster.

### <a name="event-1543-service_postrestore_failed"></a>Evento 1543: SERVICE_POSTRESTORE_FAILED

L'operazione di ripristino dei dati di configurazione del cluster non è riuscita. Il ripristino non è riuscito durante la fase "post-ripristino", che indica in genere che alcuni nodi che includono il cluster non sono attualmente operativi. Si consiglia di sostituire il file di dati di configurazione del cluster corrente (ClusDB) con ' %1'.

### <a name="event-1546-service_form_version_incompatible"></a>Evento 1546: SERVICE_FORM_VERSION_INCOMPATIBLE

Il nodo ' %1' non è riuscito a creare un cluster di failover. A causa di uno o più nodi che eseguono versioni incompatibili del software del servizio cluster. Se il nodo ' %1' o un altro nodo del cluster è stato aggiornato di recente, verificare di nuovo che tutti i nodi eseguano versioni compatibili del software del servizio cluster.

### <a name="event-1547-service_connect_version_incompatible"></a>Evento 1547: SERVICE_CONNECT_VERSION_INCOMPATIBLE

Il nodo ' %1' ha tentato di aggiungere un cluster di failover, ma non è riuscito a causa di incompatibilità tra le versioni del software del servizio cluster. Se il nodo ' %1' o un nodo diverso del cluster è stato aggiornato di recente, verificare che la distribuzione del cluster modificata con versioni diverse del software del servizio cluster sia supportata.

### <a name="event-1553-service_no_network_connectivity"></a>Evento 1553: SERVICE_NO_NETWORK_CONNECTIVITY

Questo nodo del cluster non ha connettività di rete. Non può partecipare al cluster finché non viene ripristinata la connettività. Eseguire la convalida guidata configurazione per verificare la configurazione di rete. Se la condizione è permanente, verificare la presenza di errori hardware o software correlati alla scheda di rete. Controllare anche la presenza di errori in qualsiasi altro componente di rete a cui è connesso il nodo, ad esempio hub, commutatori o Bridge.

### <a name="event-1554-service_network_connectivity_lost"></a>Evento 1554: SERVICE_NETWORK_CONNECTIVITY_LOST

Questo nodo del cluster ha perso tutta la connettività di rete. Non può partecipare al cluster finché non viene ripristinata la connettività. Il servizio cluster in questo nodo verrà terminato.

### <a name="event-1556-service_unhandled_exception_in_worker_thread"></a>Evento 1556: SERVICE_UNHANDLED_EXCEPTION_IN_WORKER_THREAD

Il servizio cluster ha rilevato un problema imprevisto e verrà arrestato. Codice di errore:' %2'.

### <a name="event-1561-service_nonstorage_witness_better_tag"></a>Evento 1561: SERVICE_NONSTORAGE_WITNESS_BETTER_TAG

Non è stato possibile avviare Servizio cluster perché questo nodo ha rilevato che non contiene la copia più recente dei dati di configurazione del cluster. Le modifiche apportate al cluster si sono verificate durante l'appartenenza del nodo e di conseguenza non è stato possibile ricevere gli aggiornamenti dei dati di configurazione.

#### <a name="guidance"></a>Indicazioni

Provare ad avviare il servizio cluster in tutti i nodi del cluster in modo che i nodi con la copia più recente dei dati di configurazione del cluster possano prima formare il cluster. Questo nodo sarà quindi in grado di accedere al cluster e otterrà automaticamente i dati di configurazione del cluster aggiornati. Se non sono disponibili nodi con la copia più recente dei dati di configurazione del cluster, eseguire il cmdlet di Windows PowerShell ' Start-ClusterNode-FQ '. Se si usa il parametro ForceQuorum (FQ), il servizio cluster verrà avviato e la copia del nodo dei dati di configurazione del cluster verrà contrassegnata come autorevole. La forzatura del quorum in un nodo con una copia obsoleta del database cluster può comportare modifiche della configurazione del cluster che si sono verificate durante la mancata partecipazione del nodo al cluster.

### <a name="event-1564-res_fsw_arbitratefailure"></a>Evento 1564: RES_FSW_ARBITRATEFAILURE

La risorsa di controllo della condivisione file ' %1' non è riuscita a eseguire l'Arbitrazione per la condivisione file ' %2'.
Assicurarsi che la condivisione file ' %2' esista e che sia accessibile dal cluster.

### <a name="event-1570-service_connect_authentication_failure"></a>Evento 1570: SERVICE_CONNECT_AUTHENTICATION_FAILURE

Il nodo ' %1' non è riuscito a stabilire una sessione di comunicazione durante l'aggiunta al cluster.
Questo problema è dovuto a un errore di autenticazione. Verificare che i nodi eseguano versioni compatibili del software del servizio cluster.

### <a name="event-1571-service_connect_authorizaion_failure"></a>Evento 1571: SERVICE_CONNECT_AUTHORIZAION_FAILURE

Il nodo ' %1' non è riuscito a stabilire una sessione di comunicazione durante l'aggiunta al cluster.
Questo problema è dovuto a un errore di autorizzazione. Verificare che i nodi eseguano versioni compatibili del software del servizio cluster.

### <a name="event-1572-service_netft_route_initial_failure"></a>Evento 1572: SERVICE_NETFT_ROUTE_INITIAL_FAILURE

Il nodo ' %1' non è riuscito ad aggiungere il cluster perché non è stato in grado di inviare e ricevere messaggi di rete di rilevamento errori con altri nodi del cluster. Eseguire la convalida guidata configurazione per verificare le impostazioni di rete. Verificare inoltre le regole del Windows Firewall ' cluster di failover '.

### <a name="event-1574-dm_database_unload_failed"></a>Evento 1574: DM_DATABASE_UNLOAD_FAILED

Impossibile scaricare il database del cluster di failover. Se il riavvio del servizio cluster non risolve il problema, riavviare il computer.

### <a name="event-1575-dm_database_corrupt_or_missing_fixquorum"></a>Evento 1575: DM_DATABASE_CORRUPT_OR_MISSING_FIXQUORUM

Il tentativo di avviare forzatamente il servizio cluster non è riuscito perché i dati di configurazione del cluster in questo nodo risultano mancanti o danneggiati. Per prima cosa avviare il servizio cluster in un altro nodo con una copia intatta e valida dei dati di configurazione del cluster. Quindi, ritentare l'operazione di avvio in questo nodo (che tenterà di ottenere automaticamente informazioni di configurazione valide aggiornate). Se non sono disponibili altri nodi, usare WBAdmin. msc per eseguire un ripristino dello stato del sistema del nodo per ripristinare i dati di configurazione.

### <a name="event-1593-dm_could_not_discard_changes"></a>Evento 1593: DM_COULD_NOT_DISCARD_CHANGES

Non è stato possibile scaricare il database del cluster di failover e non è stato possibile rimuovere eventuali modifiche potenzialmente errate nella memoria. Il servizio cluster tenterà di ripristinare il database mediante il recupero da un altro nodo del cluster. Se il servizio cluster non è online, riavviare il servizio cluster in questo nodo. Se il riavvio del servizio cluster non risolve il problema, riavviare il computer. Se il servizio cluster non riesce a tornare online dopo un riavvio, ripristinare il database del cluster dall'ultimo backup. Il database corrente è stato copiato in ' %1'. Se non sono disponibili backup, copiare ' %1' in ' %2' e provare ad avviare il servizio cluster. Se il servizio cluster viene quindi connesso a questo nodo, alcune modifiche alla configurazione del cluster potrebbero andare perse e il cluster potrebbe non funzionare correttamente. Eseguire la convalida guidata configurazione per verificare la configurazione del cluster e verificare che le applicazioni e i servizi ospitati siano online e funzionino correttamente.

### <a name="event-1672-event_node_quarantined"></a>Evento 1672: EVENT_NODE_QUARANTINED

Il nodo cluster ' %1' è stato messo in quarantena. Il nodo ha rilevato errori consecutivi ' %2' in un intervallo di tempo breve ed è stato rimosso dal cluster per evitare ulteriori interruzioni. Il nodo verrà messo in quarantena fino a' %3', quindi il nodo tenterà automaticamente di aggiungere di nuovo il cluster.

Per determinare i problemi relativi a questo nodo, vedere i registri eventi di sistema e dell'applicazione. Quando il problema viene risolto, la quarantena può essere cancellata manualmente per consentire il riaggiunta del nodo al cmdlet di Windows PowerShell "Start-ClusterNode – ClearQuarantine".

Nome nodo: %1

Il numero di membri consecutivi del cluster è perso: %2

L'ora di quarantena verrà cancellata automaticamente: %3

## <a name="error-events"></a>Eventi di errore

### <a name="event-1024-cp_reg_ckpt_restore_failed"></a>Evento 1024: CP_REG_CKPT_RESTORE_FAILED

Impossibile ripristinare il checkpoint del registro di sistema per la risorsa cluster ' %1' nella chiave del registro di sistema HKEY_LOCAL_MACHINE\\%2. La risorsa potrebbe non funzionare correttamente.
Assicurarsi che nessun altro processo disponga di handle aperti per le chiavi del registro di sistema nel sottoalbero del registro di sistema.

### <a name="event-1034-res_disk_missing"></a>Evento 1034: RES_DISK_MISSING

Impossibile portare in linea la risorsa disco fisico del cluster ' %1' perché non è stato possibile trovare il disco associato. La firma prevista del disco è' %2'.
Se il disco è stato sostituito o ripristinato, nella snap-in Gestione cluster di failover è possibile utilizzare la funzione di ripristino (nella finestra delle proprietà del disco) per ripristinare il disco nuovo o ripristinato. Se il disco non verrà sostituito, eliminare la risorsa disco associata.

### <a name="event-1035-res_disk_mount_failed"></a>Evento 1035: RES_DISK_MOUNT_FAILED

Mentre la risorsa disco ' %1' è stata portata online, l'accesso a uno o più volumi non è riuscito con errore ' %2'. Eseguire la convalida guidata configurazione per verificare la configurazione dell'archiviazione. Facoltativamente, potrebbe essere necessario eseguire CHKDSK per verificare l'integrità di tutti i volumi su questo disco.

### <a name="event-1037-res_disk_filesystem_failed"></a>Evento 1037: RES_DISK_FILESYSTEM_FAILED

Il file system per una o più partizioni del disco per la risorsa ' %1' potrebbe essere danneggiato. Eseguire la convalida guidata configurazione per verificare la configurazione dell'archiviazione. Facoltativamente, potrebbe essere necessario eseguire CHKDSK per verificare l'integrità di tutti i volumi su questo disco.

### <a name="event-1038-res_disk_reservation_lost"></a>Evento 1038: RES_DISK_RESERVATION_LOST

Il proprietario del disco del cluster ' %1' è stato perso in modo imprevisto da questo nodo. Eseguire la convalida guidata configurazione per verificare la configurazione dell'archiviazione.

### <a name="event-1039-res_genapp_create_failed"></a>Evento 1039: RES_GENAPP_CREATE_FAILED

Impossibile portare in linea l'applicazione generica ' %1' (con errore ' %2') durante un tentativo di creazione del processo. Possibili cause: l'applicazione potrebbe non essere presente in questo nodo, il nome del percorso potrebbe essere stato specificato in modo errato, il nome binario potrebbe essere stato specificato in modo errato.

### <a name="event-1040-res_gensvc_open_failed"></a>Evento 1040: RES_GENSVC_OPEN_FAILED

Impossibile portare in linea il servizio generico ' %1' (con errore ' %2') durante un tentativo di apertura del servizio. Le cause possibili includono: il servizio non è installato o il nome del servizio specificato non è valido.

### <a name="event-1041-res_gensvc_start_failed"></a>Evento 1041: RES_GENSVC_START_FAILED

Impossibile portare in linea il servizio generico ' %1' (con errore ' %2') durante un tentativo di avvio del servizio. Possibili cause: i parametri del servizio specificati potrebbero non essere validi.

### <a name="event-1042-res_gensvc_failed_after_start"></a>Evento 1042: RES_GENSVC_FAILED_AFTER_START

Il servizio generico ' %1' non è riuscito con errore ' %2'. Esaminare il registro eventi dell'applicazione.

### <a name="event-1044-res_ipaddr_nbt_interface_create_failed"></a>Evento 1044: RES_IPADDR_NBT_INTERFACE_CREATE_FAILED

Si è verificato un errore durante il tentativo di creare una nuova interfaccia NetBIOS mentre è in corso il reimpostazione della risorsa ' %1' in linea (codice errore ' %2'). È possibile che sia stato superato il numero massimo di nomi NetBIOS.

### <a name="event-1046-res_ipaddr_invalid_subnet"></a>Evento 1046: RES_IPADDR_INVALID_SUBNET

Impossibile portare in linea la risorsa indirizzo IP del cluster ' %1' perché il valore subnet mask non è valido. Verificare le proprietà delle risorse dell'indirizzo IP.

### <a name="event-1047-res_ipaddr_invalid_address"></a>Evento 1047: RES_IPADDR_INVALID_ADDRESS

Impossibile portare in linea la risorsa indirizzo IP del cluster ' %1' perché il valore dell'indirizzo non è valido. Verificare le proprietà delle risorse dell'indirizzo IP.

### <a name="event-1048-res_ipaddr_invalid_adapter"></a>Evento 1048: RES_IPADDR_INVALID_ADAPTER

La risorsa indirizzo IP del cluster ' %1' non è riuscita a tornare in linea. Non è stato possibile determinare i dati di configurazione per la scheda di rete corrispondente all'interfaccia di rete cluster ' %2' (codice di errore:' %3'). Verificare che la risorsa indirizzo IP sia configurata con l'indirizzo e le proprietà di rete corretti.

### <a name="event-1049-res_ipaddr_in_use"></a>Evento 1049: RES_IPADDR_IN_USE

La risorsa indirizzo IP del cluster ' %1' non può essere portata online perché nella rete è stato rilevato un indirizzo IP duplicato ' %2'. Assicurarsi che tutti gli indirizzi IP siano univoci.

### <a name="event-1050-res_netname_duplicate"></a>Evento 1050: RES_NETNAME_DUPLICATE

Impossibile portare in linea la risorsa del nome di rete del cluster ' %1' perché il nome ' %2' corrisponde al nome del nodo del cluster. Verificare che i nomi di rete siano univoci.

### <a name="event-1051-res_netname_no_ip_address"></a>Evento 1051: RES_NETNAME_NO_IP_ADDRESS

Impossibile portare in linea la risorsa del nome di rete del cluster ' %1'. Verificare che le schede di rete per le risorse degli indirizzi IP dipendenti abbiano accesso ad almeno un server DNS. In alternativa, abilitare NetBIOS per gli indirizzi IP dipendenti.

### <a name="event-1052-res_netname_cant_add_name_status"></a>Evento 1052: RES_NETNAME_CANT_ADD_NAME_STATUS

Impossibile portare in linea la risorsa del nome di rete del cluster ' %1' perché non è stato possibile aggiungere il nome al sistema. Il codice di errore associato viene archiviato nella sezione dati.

### <a name="event-1053-res_smb_cant_create_share"></a>Evento 1053: RES_SMB_CANT_CREATE_SHARE

Impossibile portare in linea la condivisione file del cluster ' %1' perché non è stato possibile creare la condivisione.

### <a name="event-1054-res_smb_share_not_found"></a>Evento 1054: RES_SMB_SHARE_NOT_FOUND

Il controllo integrità per la risorsa condivisione file ' %1' non è riuscito. Il recupero delle informazioni per la condivisione ' %2' (con ambito a nome di rete %3) ha restituito il codice di errore ' %4'. Verificare che la condivisione esista e che sia accessibile.

### <a name="event-1055-res_smb_share_failed"></a>Evento 1055: RES_SMB_SHARE_FAILED

Il controllo integrità per la risorsa condivisione file ' %1' non è riuscito. Il recupero delle informazioni per la condivisione ' %2' (con ambito al nome di rete %3) indica che la condivisione non esiste (codice errore ' %4'). Verificare che la condivisione esista e che sia accessibile.

### <a name="event-1069-rcm_resource_failure"></a>Evento 1069: RCM_RESOURCE_FAILURE

Risorsa cluster ' %1' nel ruolo del cluster ' %2' non riuscita.

### <a name="event-1069-rcm_resource_failure_with_typename"></a>Evento 1069: RCM_RESOURCE_FAILURE_WITH_TYPENAME

Risorsa cluster ' %1' di tipo ' %3' nel ruolo del cluster ' %2' non riuscita.<br><br>In base ai criteri di errore per la risorsa e il ruolo, il servizio cluster può tentare di portare online la risorsa in questo nodo oppure spostare il gruppo in un altro nodo del cluster e quindi riavviarlo. Verificare lo stato della risorsa e del gruppo usando Gestione cluster di failover o il cmdlet Get-ClusterResource di Windows PowerShell.

### <a name="event-1069-rcm_resource_failure_with_cause"></a>Evento 1069: RCM_RESOURCE_FAILURE_WITH_CAUSE

Risorsa cluster ' %1' di tipo ' %3' nel ruolo del cluster ' %2' non riuscita. Codice di errore: "%5" ("%4").<br><br>In base ai criteri di errore per la risorsa e il ruolo, il servizio cluster può tentare di portare online la risorsa in questo nodo oppure spostare il gruppo in un altro nodo del cluster e quindi riavviarlo. Verificare lo stato della risorsa e del gruppo usando Gestione cluster di failover o il cmdlet Get-ClusterResource di Windows PowerShell.

### <a name="event-1069-rcm_resource_failure_with_error_code"></a>Evento 1069: RCM_RESOURCE_FAILURE_WITH_ERROR_CODE

Risorsa cluster ' %1' di tipo ' %3' nel ruolo del cluster ' %2' non riuscita. Codice di errore:' %4'.<br><br>In base ai criteri di errore per la risorsa e il ruolo, il servizio cluster può tentare di portare online la risorsa in questo nodo oppure spostare il gruppo in un altro nodo del cluster e quindi riavviarlo. Verificare lo stato della risorsa e del gruppo usando Gestione cluster di failover o il cmdlet Get-ClusterResource di Windows PowerShell.

### <a name="event-1069-rcm_resource_failure_due_to_veto"></a>Evento 1069: RCM_RESOURCE_FAILURE_DUE_TO_VETO

La risorsa cluster ' %1' di tipo ' %3' nel ruolo del cluster ' %2' non è riuscita a causa di un tentativo di bloccare una modifica di stato necessaria in tale risorsa cluster.

### <a name="event-1077-res_ipaddr_ipv4_address_interface_failed"></a>Evento 1077: RES_IPADDR_IPV4_ADDRESS_INTERFACE_FAILED

Verifica integrità per l'interfaccia IP ' %1' (indirizzo ' %2') non riuscita (stato di ' %3'). Eseguire la convalida guidata configurazione per assicurarsi che la scheda di rete funzioni correttamente.

### <a name="event-1078-res_ipaddr_wins_address_failed"></a>Evento 1078: RES_IPADDR_WINS_ADDRESS_FAILED

Impossibile portare in linea la risorsa indirizzo IP del cluster ' %1' perché la registrazione WINS non è riuscita nell'interfaccia ' %2' con errore ' %3'. Verificare che sia stato specificato un server WINS valido e accessibile.

### <a name="event-1121-cp_crypto_ckpt_restore_failed"></a>Evento 1121: CP_CRYPTO_CKPT_RESTORE_FAILED

Non è stato possibile applicare le impostazioni crittografate per la risorsa cluster ' %1' al nome del contenitore ' %2' in questo nodo.

### <a name="event-1127-tm_event_cluster_netinterface_failed"></a>Evento 1127: TM_EVENT_CLUSTER_NETINTERFACE_FAILED

L'interfaccia di rete del cluster ' %1' per il nodo cluster ' %2' nella rete ' %3' non è riuscita. Eseguire la convalida guidata configurazione per verificare la configurazione di rete. Se la condizione è permanente, verificare la presenza di errori hardware o software correlati alla scheda di rete. Controllare anche la presenza di errori in qualsiasi altro componente di rete a cui è connesso il nodo, ad esempio hub, commutatori o Bridge.

### <a name="event-1129-tm_event_cluster_network_partitioned"></a>Evento 1129: TM_EVENT_CLUSTER_NETWORK_PARTITIONED

La rete cluster ' %1' è partizionata. Alcuni nodi del cluster di failover collegati non possono comunicare tra loro in rete. Il cluster di failover non è stato in grado di determinare il percorso dell'errore. Eseguire la convalida guidata configurazione per verificare la configurazione di rete. Se la condizione è permanente, verificare la presenza di errori hardware o software correlati alla scheda di rete. Controllare anche la presenza di errori in qualsiasi altro componente di rete a cui è connesso il nodo, ad esempio hub, commutatori o Bridge.

### <a name="event-1130-tm_event_cluster_network_down"></a>Evento 1130: TM_EVENT_CLUSTER_NETWORK_DOWN

La rete cluster ' %1' è inattiva. Nessuno dei nodi disponibili è in grado di comunicare utilizzando questa rete. Eseguire la convalida guidata configurazione per verificare la configurazione di rete. Se la condizione è permanente, verificare la presenza di errori hardware o software correlati alla scheda di rete. Controllare anche la presenza di errori in qualsiasi altro componente di rete a cui è connesso il nodo, ad esempio hub, commutatori o Bridge.

### <a name="event-1137-rcm_drain_move_failed"></a>Evento 1137: RCM_DRAIN_MOVE_FAILED

Non è stato possibile completare lo spostamento del ruolo del cluster ' %1' per lo svuotamento. L'operazione non è riuscita con codice errore %2.

### <a name="event-1138-res_smb_cant_create_dfs_root"></a>Evento 1138: RES_SMB_CANT_CREATE_DFS_ROOT

Impossibile portare in linea la risorsa di condivisione file del cluster ' %1'. Creazione della radice dello spazio dei nomi DFS non riuscita con errore ' %3'. Il problema potrebbe essere dovuto all'impossibilità di avviare il servizio ' spazio dei nomi DFS ' o alla creazione della radice DFS-N per la condivisione ' %2'.

### <a name="event-1141-res_smb_cant_init_dfs_svc"></a>Evento 1141: RES_SMB_CANT_INIT_DFS_SVC

Impossibile portare in linea la risorsa di condivisione file del cluster ' %1'. La risincronizzazione della destinazione radice DFS ' %2' non è riuscita con errore ' %3'.

### <a name="event-1142-res_smb_cant_online_dfs_root"></a>Evento 1142: RES_SMB_CANT_ONLINE_DFS_ROOT

Impossibile portare in linea la risorsa condivisione file del cluster ' %1' per lo spazio dei nomi DFS a causa dell'errore ' %2'.

### <a name="event-1182-netres_resource_stop_error"></a>Evento 1182: NETRES_RESOURCE_STOP_ERROR

Impossibile portare in linea la risorsa cluster ' %1' perché il servizio associato non è riuscito a eseguire un tentativo di riavvio. Il codice di errore è' %2'. Il servizio ha richiesto un riavvio per aggiornare i parametri. Tuttavia, il servizio non è riuscito prima che venisse arrestato e riavviato. Verificare la presenza di altri eventi associati al servizio e verificare che il servizio funzioni correttamente.

### <a name="event-1183-res_disk_invalid_mp_source_not_clustered"></a>Evento 1183: RES_DISK_INVALID_MP_SOURCE_NOT_CLUSTERED

La risorsa disco del cluster ' %1' contiene un punto di montaggio non valido. Sia i dischi di origine che quelli di destinazione associati al punto di montaggio devono essere dischi del cluster e devono essere membri dello stesso gruppo. <br>Il punto di montaggio ' %2' per il volume ' %3' fa riferimento a un disco di origine non valido. Verificare che il disco di origine sia anche un disco del cluster e nello stesso gruppo del disco di destinazione (che ospita il punto di montaggio).

### <a name="event-1191-res_netname_delete_computer_account_failed_status"></a>Evento 1191: RES_NETNAME_DELETE_COMPUTER_ACCOUNT_FAILED_STATUS

La risorsa del nome di rete del cluster ' %1' non è riuscita a eliminare l'oggetto computer associato nel dominio ' %2'. Codice di errore:' %3'. <br>Chiedere a un amministratore di dominio di eliminare manualmente l'oggetto computer dal dominio Active Directory.

### <a name="event-1192-res_netname_delete_computer_account_failed"></a>Evento 1192: RES_NETNAME_DELETE_COMPUTER_ACCOUNT_FAILED

La risorsa del nome di rete del cluster ' %1' non è riuscita a eliminare l'oggetto computer associato nel dominio ' %2' per il motivo seguente:<br>%3. <br><br>Chiedere a un amministratore di dominio di eliminare manualmente l'oggetto computer dal dominio Active Directory.

### <a name="event-1193-res_netname_add_computer_account_failed_status"></a>Evento 1193: RES_NETNAME_ADD_COMPUTER_ACCOUNT_FAILED_STATUS

La risorsa del nome di rete del cluster ' %1' non è riuscita a creare l'oggetto computer associato nel dominio ' %2' per il motivo seguente: %3.<br><br>Il codice di errore associato è: %5<br><br>
Collaborare con l'amministratore di dominio per verificare che:

- L'identità del cluster ' %4' può creare oggetti computer. Per impostazione predefinita, tutti gli oggetti computer vengono creati nel contenitore ' computer '; Se questo percorso è stato modificato, rivolgersi all'amministratore di dominio.
- La quota per gli oggetti computer non è stata raggiunta.
- Se è presente un oggetto computer esistente, verificare che l'identità del cluster ' %4' disponga dell'autorizzazione "controllo completo" per tale oggetto computer utilizzando lo strumento Active Directory utenti e computer.

### <a name="event-1194-res_netname_add_computer_account_failed"></a>Evento 1194: RES_NETNAME_ADD_COMPUTER_ACCOUNT_FAILED

La risorsa del nome di rete del cluster ' %1' non è riuscita a creare l'oggetto computer associato nel dominio ' %2' durante: %3.<br><br>Il testo per il codice di errore associato è: %4

Collaborare con l'amministratore di dominio per verificare che:

- Per l'identità del cluster ' %5' sono disponibili autorizzazioni Crea oggetti computer. Per impostazione predefinita, tutti gli oggetti computer vengono creati nello stesso contenitore dell'identità del cluster ' %5'.
- La quota per gli oggetti computer non è stata raggiunta.
- Se è presente un oggetto computer esistente, verificare che l'identità del cluster ' %5' disponga dell'autorizzazione "controllo completo" per tale oggetto computer utilizzando lo strumento Active Directory utenti e computer.

### <a name="event-1195-res_netname_dns_registration_failed_status"></a>Evento 1195: RES_NETNAME_DNS_REGISTRATION_FAILED_STATUS

La risorsa nome rete cluster ' %1' non è riuscita a registrare uno o più nomi DNS associati. Codice di errore:' %2'. Verificare che le schede di rete associate alle risorse degli indirizzi IP dipendenti siano configurate con accesso ad almeno un server DNS.

### <a name="event-1196-res_netname_dns_registration_failed"></a>Evento 1196: RES_NETNAME_DNS_REGISTRATION_FAILED

La risorsa del nome di rete del cluster ' %1' non è riuscita a registrare uno o più nomi DNS associati per il motivo seguente:<br>%2.<br><br>Verificare che le schede di rete associate alle risorse degli indirizzi IP dipendenti siano configurate con almeno un server DNS accessibile.

### <a name="event-1205-rcm_event_group_failed_online_offline"></a>Evento 1205: RCM_EVENT_GROUP_FAILED_ONLINE_OFFLINE

Il Servizio cluster non è riuscito a portare completamente online il ruolo del cluster ' %1' o offline. Una o più risorse possono trovarsi in uno stato di errore. Questa operazione può influisca sulla disponibilità del ruolo del cluster.

### <a name="event-1206-res_netname_update_computer_account_failed_status"></a>Evento 1206: RES_NETNAME_UPDATE_COMPUTER_ACCOUNT_FAILED_STATUS

Impossibile aggiornare l'oggetto computer associato alla risorsa del nome di rete del cluster ' %1' nel dominio ' %2'. Codice di errore:' %3'. L'identità del cluster ' %4' potrebbe non avere le autorizzazioni necessarie per aggiornare l'oggetto. Collaborare con l'amministratore di dominio per assicurarsi che l'identità del cluster possa aggiornare gli oggetti computer nel dominio.

### <a name="event-1207-res_netname_update_computer_account_failed"></a>Evento 1207: RES_NETNAME_UPDATE_COMPUTER_ACCOUNT_FAILED

Impossibile aggiornare l'oggetto computer associato alla risorsa del nome di rete del cluster ' %1' nel dominio ' %2' durante il <br>operazione %3.<br><br>Il testo per il codice di errore associato è: %4<br> <br>L'identità del cluster ' %5' potrebbe non avere le autorizzazioni necessarie per aggiornare l'oggetto. Collaborare con l'amministratore di dominio per assicurarsi che l'identità del cluster possa aggiornare gli oggetti computer nel dominio.

### <a name="event-1208-res_disk_invalid_mp_target_not_clustered"></a>Evento 1208: RES_DISK_INVALID_MP_TARGET_NOT_CLUSTERED

La risorsa disco del cluster ' %1' contiene un punto di montaggio non valido. Sia i dischi di origine che quelli di destinazione associati al punto di montaggio devono essere dischi del cluster e devono essere membri dello stesso gruppo. <br>Il punto di montaggio ' %2' per il volume ' %3' fa riferimento a un disco di destinazione non valido. Verificare che il disco di destinazione sia anche un disco del cluster e nello stesso gruppo del disco di origine (che ospita il punto di montaggio).

### <a name="event-1211-res_netname_no_writeable_dc_status"></a>Evento 1211: RES_NETNAME_NO_WRITEABLE_DC_STATUS

Impossibile portare in linea la risorsa del nome di rete del cluster ' %1'. Tentativo di individuazione di un controller di dominio scrivibile (nel dominio %2) per la creazione o l'aggiornamento di un oggetto computer associato alla risorsa non riuscito. Codice di errore:' %3'.
Verificare che un controller di dominio scrivibile sia accessibile a questo nodo all'interno del dominio configurato. Verificare inoltre che il server DNS sia in esecuzione per risolvere il nome del controller di dominio.

### <a name="event-1212-res_netname_no_writeable_dc"></a>Evento 1212: RES_NETNAME_NO_WRITEABLE_DC

Impossibile portare in linea la risorsa del nome di rete del cluster ' %1'. Il tentativo di individuare un controller di dominio scrivibile (nel dominio %2) per creare o aggiornare un oggetto computer associato alla risorsa non è riuscito per il motivo seguente:<br>%3.<br><br> Codice di errore:' %4'. Verificare che un controller di dominio scrivibile sia accessibile a questo nodo all'interno del dominio configurato. Verificare inoltre che il server DNS sia in esecuzione per risolvere il nome del controller di dominio.

### <a name="event-1213-res_netname_rename_restore_failed"></a>Evento 1213: RES_NETNAME_RENAME_RESTORE_FAILED

La risorsa del nome di rete del cluster ' %1' non è stata in grado di rinominare completamente l'oggetto computer associato sul controller di dominio ' %2'. Anche il tentativo di rinominare l'oggetto computer dal nuovo nome ' %3' al nome originale ' %4' non è riuscito. Codice di errore:' %5'. Questa operazione può influire sulla connettività client fino a quando il nome di rete e il nome dell'oggetto computer associato non sono coerenti. Contattare l'amministratore di dominio per rinominare manualmente l'oggetto computer.

### <a name="event-1214-res_netname_cant_add_name2"></a>Evento 1214: RES_NETNAME_CANT_ADD_NAME2

Impossibile registrare la risorsa del nome di rete del cluster con NetBIOS. <br><br>Nome rete:' %3' <br>Motivo dell'errore: %2 <br>Nome risorsa:' %1' <br><br> Eseguire nbtstat per il nome di rete per assicurarsi che il nome non sia già registrato con NetBIOS.

### <a name="event-1215-res_netname_not_registered_with_rdr"></a>Evento 1215: RES_NETNAME_NOT_REGISTERED_WITH_RDR

La risorsa del nome di rete del cluster ' %1' non è riuscita a verificare l'integrità. Il nome di rete ' %2' non è più registrato in questo nodo. Codice di errore:' %3'. Verificare la presenza di errori hardware o software correlati alla scheda di rete. Inoltre, è possibile eseguire la convalida guidata configurazione per verificare la configurazione di rete.

### <a name="event-1218-res_netname_online_rename_recovery_missing_account"></a>Evento 1218: RES_NETNAME_ONLINE_RENAME_RECOVERY_MISSING_ACCOUNT

La risorsa del nome di rete del cluster ' %1' non è riuscita a eseguire un'operazione di modifica del nome (il tentativo di modificare il nome originale ' %3' nel nome ' %4'). Impossibile trovare l'oggetto computer nel controller di dominio ' %2' (in cui è stato creato). Verrà effettuato un tentativo di ricreare l'oggetto computer la volta successiva che la risorsa verrà portata online. Inoltre, rivolgersi all'amministratore di dominio per verificare che l'oggetto computer esista nel dominio.

### <a name="event-1219-res_netname_online_rename_dc_not_found"></a>Evento 1219: RES_NETNAME_ONLINE_RENAME_DC_NOT_FOUND

La risorsa del nome di rete del cluster ' %1' non è riuscita a eseguire un'operazione di modifica del nome.
Impossibile contattare il controller di dominio ' %2' in cui è stato rinominato l'oggetto computer ' %3'. Codice di errore:' %4'. Verificare che sia possibile accedere a un controller di dominio scrivibile e verificare la presenza di eventuali problemi di connettività.

### <a name="event-1220-res_netname_online_rename_recovery_failed"></a>Evento 1220: RES_NETNAME_ONLINE_RENAME_RECOVERY_FAILED

L'account computer per la risorsa ' %1' non è stato rinominato da %2 a %3 utilizzando il controller di dominio %4. Il codice di errore associato viene archiviato nella sezione dati.<br>
<br>L'account computer per questa risorsa è in corso di ridenominazione e non è stato completato. Questa operazione è stata rilevata durante il processo online per questa risorsa.
Per eseguire il ripristino, l'account computer deve essere rinominato in base al valore corrente della proprietà Name, ad esempio, il nome presentato sulla rete.<br> <br>Il controller di dominio in cui è stato eseguito il tentativo di ridenominazione potrebbe non essere disponibile; in tal caso, attendere che il controller di dominio sia nuovamente disponibile. Il controller di dominio potrebbe negare l'accesso all'account; Dopo la risoluzione dell'accesso, provare a riportare il nome online.<br> <br>Se ciò non è possibile, disabilitare e riabilitare l'autenticazione Kerberos e verrà effettuato un tentativo di trovare l'account computer in un controller di dominio diverso. È necessario impostare la proprietà Name su %2 per utilizzare l'account computer esistente.

### <a name="event-1223-res_ipaddr_invalid_network_role"></a>Evento 1223: RES_IPADDR_INVALID_NETWORK_ROLE

Impossibile portare in linea la risorsa indirizzo IP del cluster ' %1' perché la rete cluster ' %2' non è configurata per consentire l'accesso client. Usare il snap-in Gestione cluster di failover per verificare le proprietà configurate della rete cluster.

### <a name="event-1226-res_netname_tcb_not_held"></a>Evento 1226: RES_NETNAME_TCB_NOT_HELD

Per la risorsa del nome di rete ' %1' (con il nome di rete associato ' %2') è abilitato il supporto per l'autenticazione Kerberos. Non è stato possibile aggiungere le credenziali richieste a LSA. il codice di errore associato ' %3' indica che in genere sono necessari privilegi insufficienti per questa operazione. Il privilegio richiesto è "Trusted computing base" e deve essere abilitato localmente in ogni nodo che comprende il cluster.

### <a name="event-1227-res_netname_lsa_error"></a>Evento 1227: RES_NETNAME_LSA_ERROR

Per la risorsa del nome di rete ' %1' (con il nome di rete associato ' %2') è abilitato il supporto per l'autenticazione Kerberos. Non è stato possibile aggiungere le credenziali richieste a LSA. il codice di errore associato è' %3'.

### <a name="event-1228-res_netname_clone_failure"></a>Evento 1228: RES_NETNAME_CLONE_FAILURE

La risorsa del nome di rete del cluster ' %1' ha rilevato un errore durante l'abilitazione del nome di rete in questo nodo. Il motivo dell'errore è stato: <br> ' %2'.<br><br> Codice di errore:' %3'. <br><br> Per riprovare, è possibile che la risorsa nome di rete sia offline e online.

### <a name="event-1229-res_netname_no_ips_for_dns"></a>Evento 1229: RES_NETNAME_NO_IPS_FOR_DNS

La risorsa del nome di rete del cluster ' %1' non è stata in grado di identificare gli indirizzi IP da registrare con un server DNS. Assicurarsi che esistano una o più reti abilitate per l'uso del cluster con l'impostazione ' Consenti ai client di connettersi tramite questa rete ' e che ogni nodo disponga di un indirizzo IP valido configurato per le reti.

### <a name="event-1230-rcm_deadlock_or_crash_detected"></a>Evento 1230: RCM_DEADLOCK_OR_CRASH_DETECTED

Un componente del server non ha risposto in modo tempestivo. Questa operazione ha causato il superamento della soglia di timeout della risorsa cluster ' %1' (tipo di risorsa ' %2', DLL ' %3'). Come parte del rilevamento dell'integrità del cluster, verranno eseguite le azioni di ripristino.
Il cluster tenterà di eseguire automaticamente il ripristino terminando e riavviando il processo di Resource Hosting Subsystem (RHS) che esegue questa risorsa. Verificare che l'infrastruttura sottostante, ad esempio archiviazione, rete o servizi, associata alla risorsa funzioni correttamente.

### <a name="event-1230-rcm_resource_control_deadlock_detected"></a>Evento 1230: RCM_RESOURCE_CONTROL_DEADLOCK_DETECTED

Un componente del server non ha risposto in modo tempestivo. Questa operazione ha causato il superamento della soglia di timeout della risorsa cluster ' %1' (tipo di risorsa ' %2', DLL ' %3') durante l'elaborazione del codice di controllo ' %4;'. Come parte del rilevamento dell'integrità del cluster, verranno eseguite le azioni di ripristino. Il cluster tenterà di eseguire automaticamente il ripristino terminando e riavviando il processo di Resource Hosting Subsystem (RHS) che esegue questa risorsa. Verificare che l'infrastruttura sottostante, ad esempio archiviazione, rete o servizi, associata alla risorsa funzioni correttamente.

### <a name="event-1231-res_netname_logon_failure"></a>Evento 1231: RES_NETNAME_LOGON_FAILURE

La risorsa del nome di rete del cluster ' %1' ha rilevato un errore durante l'accesso al dominio. Il motivo dell'errore è stato: <br> ' %2'.<br><br> Codice di errore:' %3'.
<br><br> Verificare che il controller di dominio sia accessibile a questo nodo all'interno del dominio configurato.

### <a name="event-1232-res_genscript_timeout"></a>Evento 1232: RES_GENSCRIPT_TIMEOUT

Il punto di ingresso ' %2' nella risorsa script generico del cluster ' %1' non ha completato l'esecuzione in modo tempestivo. Questo potrebbe essere dovuto a un ciclo infinito o ad altri problemi che potrebbero causare un'attesa infinita. In alternativa, il valore di timeout in sospeso specificato potrebbe essere troppo breve per questa risorsa. Esaminare il punto di ingresso dello script ' %2' per assicurarsi che tutte le possibili attese infinite nel codice di script siano state corrette. Si consiglia quindi di aumentare il valore di timeout in sospeso se ritenuto necessario.

### <a name="event-1233-res_genscript_hangmode"></a>Evento 1233: RES_GENSCRIPT_HANGMODE

Risorsa script generico cluster ' %1': la richiesta di esecuzione dell'operazione ' %2' non verrà elaborata. Ciò è dovuto a un tentativo non riuscito in precedenza di eseguire il punto di ingresso ' %3' in modo tempestivo. Esaminare il codice di script per questo punto di ingresso per assicurarsi che non esista un ciclo infinito o che altri problemi possano causare un'attesa infinita. Si consiglia quindi di aumentare il valore di timeout in sospeso della risorsa se ritenuto necessario.

### <a name="event-1234-cluster_event_account_missing_privs"></a>Evento 1234: CLUSTER_EVENT_ACCOUNT_MISSING_PRIVS

Il Servizio cluster ha rilevato che per l'account del servizio mancano uno o più dei privilegi necessari. L'elenco dei privilegi mancanti è:' %1' e non è attualmente concesso all'account del servizio. Utilizzare ' SC. exe qprivs clussvc ' per verificare i privilegi del Servizio cluster (ClusSvc). Controllare inoltre se sono presenti criteri di sicurezza o criteri di gruppo in Active Directory Domain Services che potrebbero avere modificato i privilegi predefiniti. Digitare il comando seguente per concedere al Servizio cluster i privilegi necessari per il corretto funzionamento:

```
sc.exe privs
clussvc
SeBackupPrivilege/SeRestorePrivilege/SeIncreaseQuotaPrivilege/SeIncreaseBasePriorityPrivilege/SeTcbPrivilege/SeDebugPrivilege/SeSecurityPrivilege/SeAuditPrivilege/SeImpersonatePrivilege/SeChangeNotifyPrivilege/SeIncreaseWorkingSetPrivilege/SeManageVolumePrivilege/SeCreateSymbolicLinkPrivilege/SeLoadDriverPrivilege
```

### <a name="event-1242-res_ipaddr_lease_expired"></a>Evento 1242: RES_IPADDR_LEASE_EXPIRED

Il lease dell'indirizzo IP ' %2' associato alla risorsa indirizzo IP del cluster ' %1' è scaduto o sta per scadere e attualmente non può essere rinnovato. Verificare che il server DHCP associato sia accessibile e configurato correttamente per rinnovare il lease in questo indirizzo IP.

### <a name="event-1245-res_ipaddr_lease_renewal_failed"></a>Evento 1245: RES_IPADDR_LEASE_RENEWAL_FAILED

La risorsa indirizzo IP del cluster ' %1' non è riuscita a rinnovare il lease per l'indirizzo IP ' %2'.
Verificare che il server DHCP sia accessibile e configurato correttamente per rinnovare il lease in questo indirizzo IP.

### <a name="event-1250-rcm_resource_embedded_failure"></a>Evento 1250: RCM_RESOURCE_EMBEDDED_FAILURE

La risorsa cluster ' %1' nel ruolo del cluster ' %2' ha ricevuto una notifica di stato critico. Per una macchina virtuale indica che un'applicazione o un servizio all'interno della macchina virtuale si trova in uno stato non integro. Verificare la funzionalità del servizio o dell'applicazione monitorata all'interno della macchina virtuale.

### <a name="event-1254-rcm_group_terminal_failure"></a>Evento 1254: RCM_GROUP_TERMINAL_FAILURE

Il ruolo del cluster ' %1' ha superato la soglia di failover. Il numero di tentativi di failover configurato è stato esaurito entro il periodo di failover del tempo assegnato e verrà lasciato in uno stato di errore. Non verrà effettuato alcun tentativo aggiuntivo di portare online il ruolo o di eseguirne il failover in un altro nodo del cluster.
Controllare gli eventi associati all'errore. Una volta risolti i problemi che causano l'errore, il ruolo può essere portato online manualmente oppure il cluster può tentare di riportarlo online dopo il periodo di ritardo del riavvio.

### <a name="event-1255-rcm_resource_network_failure"></a>Evento 1255: RCM_RESOURCE_NETWORK_FAILURE

La risorsa cluster ' %1' nel ruolo del cluster ' %2' ha ricevuto una notifica di stato critico. Per una macchina virtuale indica che una rete critica della macchina virtuale è in uno stato non integro. Verificare la connettività di rete della macchina virtuale e le reti virtuali configurate per l'uso nella macchina virtuale.

### <a name="event-1256-res_netname_dns_registration_failed_dynamic_dns_zone"></a>Evento 1256: RES_NETNAME_DNS_REGISTRATION_FAILED_DYNAMIC_DNS_ZONE

La risorsa del nome di rete cluster non è riuscita a registrare uno o più nomi DNS associati perché la zona DNS corrispondente non accetta aggiornamenti dinamici.<br><br>Nome rete cluster:' %1'<br>Zona DNS:' %2'

#### <a name="guidance"></a>Indicazioni

Verificare che il DNS sia configurato come zona DNS dinamica. Se il server DNS non accetta aggiornamenti dinamici deselezionare l'opzione ' registra gli indirizzi della connessione in DNS ' nelle proprietà della scheda di rete.

### <a name="event-1257-res_netname_dns_registration_failed_secure_dns_zone"></a>Evento 1257: RES_NETNAME_DNS_REGISTRATION_FAILED_SECURE_DNS_ZONE

La risorsa del nome di rete cluster non è riuscita a registrare uno o più nomi DNS associati perché l'accesso per aggiornare la zona DNS protetta è stato negato.<br><br>Nome rete cluster:' %1'<br>Zona DNS:' %2'<br><br>Verificare che all'oggetto nome cluster (oggetto nome cluster) siano concesse le autorizzazioni per la zona DNS protetta.

### <a name="event-1258-res_netname_dns_registration_failed_timeout"></a>Evento 1258: RES_NETNAME_DNS_REGISTRATION_FAILED_TIMEOUT

La risorsa del nome di rete cluster non è riuscita a registrare uno o più nomi DNS associati perché non è stato possibile raggiungere il server DNS.<br><br>Nome rete cluster:' %1'<br>Zona DNS:' %2'<br>Server DNS:' %3'<br><br>Verificare che le schede di rete associate alle risorse degli indirizzi IP dipendenti siano configurate con almeno un server DNS accessibile.

### <a name="event-1259-res_netname_dns_registration_failed_cleanup"></a>Evento 1259: RES_NETNAME_DNS_REGISTRATION_FAILED_CLEANUP

La risorsa del nome di rete cluster non è riuscita a registrare uno o più nomi DNS associati perché il servizio cluster non è riuscito a pulire i record esistenti corrispondenti al nome di rete.<br><br>Nome rete cluster:' %1'<br>Zona DNS:' %2'<br><br>Verificare che all'oggetto nome cluster (oggetto nome cluster) siano concesse le autorizzazioni per la zona DNS protetta.

### <a name="event-1260-res_netname_dns_registration_modify_failed"></a>Evento 1260: RES_NETNAME_DNS_REGISTRATION_MODIFY_FAILED

La risorsa nome di rete cluster non è riuscita a modificare la registrazione DNS.<br><br>Nome rete cluster:' %1'<br>Codice errore:' %2'

#### <a name="guidance"></a>Indicazioni

Verificare che le schede di rete associate alle risorse degli indirizzi IP dipendenti siano configurate con accesso ad almeno un server DNS.

### <a name="event-1261-res_netname_dns_registration_modify_failed_status"></a>Evento 1261: RES_NETNAME_DNS_REGISTRATION_MODIFY_FAILED_STATUS

La risorsa nome di rete cluster non è riuscita a modificare la registrazione DNS.<br><br>Nome rete cluster:' %1'<br>Motivo:' %2'

#### <a name="guidance"></a>Indicazioni

Verificare che le schede di rete associate alle risorse degli indirizzi IP dipendenti siano configurate con accesso ad almeno un server DNS.

### <a name="event-1262-res_netname_dns_registration_publish_ptr_failed"></a>Evento 1262: RES_NETNAME_DNS_REGISTRATION_PUBLISH_PTR_FAILED

La risorsa nome di rete cluster non è riuscita a pubblicare il record PTR nella zona DNS di ricerca inversa.<br><br>Nome rete cluster:' %1'<br>Codice errore:' %2'

#### <a name="guidance"></a>Indicazioni

Verificare che le schede di rete associate alle risorse degli indirizzi IP dipendenti siano configurate con accesso ad almeno un server DNS e che la zona di ricerca inversa DNS esista.

### <a name="event-1264-res_netname_dns_registration_publish_ptr_failed_status"></a>Evento 1264: RES_NETNAME_DNS_REGISTRATION_PUBLISH_PTR_FAILED_STATUS

La risorsa nome di rete cluster non è riuscita a pubblicare il record PTR nella zona DNS di ricerca inversa.<br><br>Nome rete cluster:' %1'<br>Motivo:' %2'

#### <a name="guidance"></a>Indicazioni

Verificare che le schede di rete associate alle risorse degli indirizzi IP dipendenti siano configurate con accesso ad almeno un server DNS e che la zona di ricerca inversa DNS esista.

### <a name="event-1265-res_type_control_timed_out"></a>Evento 1265: RES_TYPE_CONTROL_TIMED_OUT

Timeout del tipo di risorsa cluster ' %1' durante l'elaborazione del codice di controllo %2. Il cluster tenterà di eseguire automaticamente il ripristino terminando e riavviando il processo di Resource Hosting Subsystem (RHS) che stava elaborando la chiamata.

### <a name="event-1289-netft_adapter_not_found"></a>Evento 1289: NETFT_ADAPTER_NOT_FOUND

Il servizio cluster non è riuscito ad accedere alla scheda di rete ' %1'. Verificare che le altre schede di rete funzionino correttamente e controllare Gestione dispositivi degli errori associati all'adapter ' %1'. Se la configurazione per l'adapter ' %1' è stata modificata, potrebbe essere necessario reinstallare la funzionalità clustering di failover nel computer.

### <a name="event-1360-res_ipaddr_invalid_network"></a>Evento 1360: RES_IPADDR_INVALID_NETWORK

La risorsa indirizzo IP del cluster ' %1' non è riuscita a tornare in linea. Verificare che la proprietà di rete ' %2' corrisponda a un nome di rete del cluster o che la proprietà Address ' %3' corrisponda a una delle subnet in una rete cluster. Se si tratta di un tipo di indirizzo IPv6, verificare che la rete del cluster che corrisponde a questa risorsa includa almeno un prefisso IPv6 che non sia locale o tunnel di collegamento.

### <a name="event-1361-res_ipaddr_missing_dependant"></a>Evento 1361: RES_IPADDR_MISSING_DEPENDANT

La risorsa di indirizzo del tunnel IPv6' %1' non è riuscita a tornare online perché non dipende da una risorsa indirizzo IP (IPv4). È necessaria una dipendenza da almeno una risorsa indirizzo IP (IPv4).

### <a name="event-1362-res_ipaddr_missing_data"></a>Evento 1362: RES_IPADDR_MISSING_DATA

La risorsa indirizzo IP del cluster ' %1' non è riuscita a tornare online perché non è stato possibile leggere la proprietà' %2'. Verificare che la risorsa sia configurata correttamente.

### <a name="event-1363-res_ipaddr_no_isatap_support"></a>Evento 1363: RES_IPADDR_NO_ISATAP_SUPPORT

La risorsa di indirizzo del tunnel IPv6' %1' non è riuscita a tornare in linea. La rete cluster ' %2' associata alla risorsa indirizzo IP (IPv4) dipendente ' %3' non supporta il tunneling ISATAP. Verificare che la rete cluster supporti il tunneling ISATAP.

### <a name="event-1540-service_backup_noquorum"></a>Evento 1540: SERVICE_BACKUP_NOQUORUM

L'operazione di backup per i dati di configurazione del cluster è stata interrotta perché il quorum per il cluster non è stato ancora raggiunto. Ripetere l'operazione di backup dopo il raggiungimento del quorum da parte del cluster.

### <a name="event-1554-service_restore_invaliduser"></a>Evento 1554: SERVICE_RESTORE_INVALIDUSER

L'operazione di ripristino per i dati di configurazione del cluster non è riuscita. Questa operazione era dovuta a privilegi insufficienti associati all'account utente che esegue il ripristino. Verificare che l'account utente disponga dei privilegi di amministratore locale.

### <a name="event-1557-service_witness_attach_failed"></a>Evento 1557: SERVICE_WITNESS_ATTACH_FAILED

Servizio cluster non è riuscito ad aggiornare i dati di configurazione del cluster nella risorsa server di controllo del mirroring. Assicurarsi che la risorsa del server di controllo del mirroring sia online e accessibile.

### <a name="event-1559-res_witness_new_node_conflict"></a>Evento 1559: RES_WITNESS_NEW_NODE_CONFLICT

La condivisione file ' %1' associata alla risorsa di controllo di condivisione file è attualmente ospitata dal server ' %2'. Il server ' %2' è stato appena aggiunto come nuovo nodo nel cluster di failover. Non è consigliabile ospitare la condivisione file di controllo da qualsiasi nodo che comprende lo stesso cluster. Scegliere una condivisione file di controllo non ospitata da alcun nodo nello stesso cluster e modificare di conseguenza le impostazioni della risorsa di controllo della condivisione file.

### <a name="event-1560-res_smb_share_conflict"></a>Evento 1560: RES_SMB_SHARE_CONFLICT

La risorsa di condivisione file del cluster ' %1' ha rilevato conflitti tra le cartelle condivise. Di conseguenza, alcune di queste cartelle condivise potrebbero non essere accessibili. Per correggere questa situazione, assicurarsi che più cartelle condivise non abbiano lo stesso nome di condivisione.

### <a name="event-1563-res_fsw_onlinefailure"></a>Evento 1563: RES_FSW_ONLINEFAILURE

La risorsa di controllo di condivisione file ' %1' non è riuscita a tornare in linea. Assicurarsi che la condivisione file ' %2' esista e che sia accessibile dal cluster.

### <a name="event-1566-res_netname_timedout"></a>Evento 1566: RES_NETNAME_TIMEDOUT

Impossibile portare in linea la risorsa del nome di rete del cluster ' %1'. La risorsa del nome di rete è stata terminata dal sottosistema host di risorse perché non ha completato un'operazione in un tempo accettabile. Si è verificato il timeout dell'operazione durante l'esecuzione di:<br> ' %2'

### <a name="event-1567-service_failed_to_change_log_size"></a>Evento 1567: SERVICE_FAILED_TO_CHANGE_LOG_SIZE

Servizio cluster non è stato possibile modificare le dimensioni del log di traccia. Verificare l'impostazione ClusterLogSize con il cmdlet di PowerShell "Get-cluster \| Format-List \*". Inoltre, utilizzare lo snap-in Performance Monitor per verificare le impostazioni della sessione di traccia eventi per FailoverClustering.

### <a name="event-1567-res_vipaddr_address_interface_failed"></a>Evento 1567: RES_VIPADDR_ADDRESS_INTERFACE_FAILED

Verifica integrità per l'interfaccia IP ' %1' (indirizzo ' %2') non riuscita (stato di ' %3'). Verificare la presenza di errori hardware o software correlati alle schede di rete fisiche o virtuali.

### <a name="event-1568-res_cloud_witness_cant_communicate_to_azure"></a>Evento 1568: RES_CLOUD_WITNESS_CANT_COMMUNICATE_TO_AZURE

La risorsa di controllo cloud non è riuscita a raggiungere i servizi di archiviazione Microsoft Azure.<br><br>Risorsa cluster: %1 <br>Nodo cluster: %2 

#### <a name="guidance"></a>Indicazioni

La causa potrebbe essere la comunicazione di rete tra il nodo del cluster e il servizio Microsoft Azure bloccato. Verificare la connettività Internet del nodo al Microsoft Azure. Connettersi al portale di Microsoft Azure e verificare che l'account di archiviazione esista.

### <a name="event-1569-service_using_restricted_network"></a>Evento 1569: SERVICE_USING_RESTRICTED_NETWORK

La rete ' %1' che è stata disabilitata per l'uso del cluster di failover, è stata trovata come unica rete attualmente possibile che il nodo ' %2' può usare per comunicare con gli altri nodi del cluster. Questo può influisca sulla capacità del nodo di partecipare al cluster. Verificare la connettività di rete del nodo ' %2' e abilitare almeno una rete per la comunicazione del cluster. Eseguire la convalida guidata configurazione per verificare la configurazione di rete.

### <a name="event-1569-res_cloud_witness_token_expired"></a>Evento 1569: RES_CLOUD_WITNESS_TOKEN_EXPIRED

Non è stato possibile eseguire l'autenticazione della risorsa cloud Witness con Microsoft Azure servizi di archiviazione. È stato restituito un errore di accesso negato durante il tentativo di contattare il Microsoft Azure account di archiviazione. <br><br>Risorsa cluster: %1 

#### <a name="guidance"></a>Indicazioni

La chiave di accesso dell'account di archiviazione potrebbe non essere più valida. Usare la configurazione guidata quorum del cluster nel Gestione cluster di failover o il cmdlet di Windows PowerShell set-ClusterQuorum per configurare la risorsa di cloud Witness con la chiave di accesso dell'account di archiviazione aggiornata.

### <a name="event-1573-service_form_witness_failed"></a>Evento 1573: SERVICE_FORM_WITNESS_FAILED

Il nodo ' %1' non è riuscito a creare un cluster. Questo perché il server di controllo del mirroring non era accessibile. Assicurarsi che la risorsa del server di controllo del mirroring sia online e disponibile.

### <a name="event-1580-res_netname_dns_registration_secure_zone_failed"></a>Evento 1580: RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_FAILED

La risorsa del nome di rete del cluster ' %1' non è riuscita a registrare il nome ' %2' sulla scheda ' %4' in una zona DNS protetta. Questo è dovuto a un record esistente con questo nome e l'identità del cluster non dispone dei privilegi sufficienti per aggiornare il record. Codice di errore:' %3'. Contattare l'amministratore del server DNS per verificare che l'identità del cluster disponga delle autorizzazioni per il record DNS ' %2'.

### <a name="event-1580-res_netname_dns_registration_secure_zone_failed"></a>Evento 1580: RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_FAILED

La risorsa del nome di rete del cluster ' %1' non è riuscita a registrare il nome ' %2' sulla scheda ' %4' in una zona DNS protetta. Questo è dovuto a un record esistente con questo nome e l'identità del cluster non dispone dei privilegi sufficienti per aggiornare il record. Codice di errore:' %3'. Contattare l'amministratore del server DNS per verificare che l'identità del cluster disponga delle autorizzazioni per il record DNS ' %2'.

### <a name="event-1585-res_fileserver_fscheck_srvsvc_stopped"></a>Evento 1585: RES_FILESERVER_FSCHECK_SRVSVC_STOPPED

Il controllo integrità del cluster file server risorsa ' %1' non è riuscito. Questo problema è dovuto al fatto che il servizio Server non è stato avviato. Usare Server Manager per verificare lo stato del servizio server in questo nodo del cluster.

### <a name="event-1586-res_fileserver_fscheck_scoped_name_not_registered"></a>Evento 1586: RES_FILESERVER_FSCHECK_SCOPED_NAME_NOT_REGISTERED

Il controllo integrità del cluster file server risorsa ' %1' non è riuscito. Questo perché alcune delle cartelle condivise sono inaccessibili. Verificare che le cartelle siano accessibili dai client. Inoltre, verificare lo stato del servizio server in questo nodo del cluster utilizzando Server Manager e cercare altri eventi correlati al servizio server in questo nodo del cluster. Potrebbe essere necessario riavviare la risorsa del nome di rete ' %2' in questo ruolo del cluster.

### <a name="event-1587-res_fileserver_fscheck_failed"></a>Evento 1587: RES_FILESERVER_FSCHECK_FAILED

Il controllo integrità del cluster file server risorsa ' %1' non è riuscito. Questo perché alcune delle cartelle condivise sono inaccessibili. Verificare che le cartelle siano accessibili dai client. Inoltre, verificare lo stato del servizio server in questo nodo del cluster utilizzando Server Manager e cercare altri eventi correlati al servizio server in questo nodo del cluster.

### <a name="event-1588-res_fileserver_share_cant_add"></a>Evento 1588: RES_FILESERVER_SHARE_CANT_ADD

Impossibile portare in linea la risorsa file server cluster ' %1'. La risorsa non è riuscita a creare la condivisione file ' %2' associata al nome di rete ' %3'. Codice di errore:' %4'. Verificare che le cartelle esistano e siano accessibili. Inoltre, verificare lo stato del servizio server in questo nodo del cluster utilizzando Server Manager e cercare altri eventi correlati in questo nodo del cluster. Potrebbe essere necessario riavviare la risorsa del nome di rete ' %3' in questo ruolo del cluster.

### <a name="event-1600-clusapi_create_cannot_set_ad_dacl"></a>Evento 1600: CLUSAPI_CREATE_CANNOT_SET_AD_DACL

Servizio cluster non è stato possibile impostare le autorizzazioni per l'oggetto computer del cluster ' %1'. Contattare l'amministratore di rete per controllare il descrittore di sicurezza del cluster dell'oggetto computer in Active Directory, verificare che il DACL non sia troppo grande e rimuovere eventuali ACE aggiuntive superflue nell'oggetto, se necessario.

### <a name="event-1603-res_fileserver_clone_failed"></a>Evento 1603: RES_FILESERVER_CLONE_FAILED

Impossibile avviare il file server perché non è stata trovata una dipendenza prevista dalla risorsa ' nome di rete ' oppure non è stata configurata correttamente. Errore = 0x %2.

### <a name="event-1606-res_disk_cno_check_failed"></a>Evento 1606: RES_DISK_CNO_CHECK_FAILED

La risorsa disco del cluster ' %1' contiene un volume protetto da BitLocker,' %2', ma per questo volume, l'account del nome del cluster Active Directory (detto anche oggetto nome cluster o oggetto nome cluster) non è una protezione BitLocker per il volume. Questa operazione è necessaria per i volumi protetti da BitLocker. Per risolvere il problema, rimuovere innanzitutto il disco dal cluster. Usare quindi lo strumento da riga di comando Manage-bde. exe per aggiungere il nome del cluster come protezione ADAccountOrGroup usando il formato Domain\\clustername\$ per il nome del cluster. Quindi, aggiungere di nuovo il disco al cluster. Per ulteriori informazioni, vedere la documentazione di Manage-bde. exe

### <a name="event-1607-res_disk_cno_unlock_failed"></a>Evento 1607: RES_DISK_CNO_UNLOCK_FAILED

La risorsa disco del cluster ' %1' non è riuscita a sbloccare il volume protetto da BitLocker ' %2'. L'oggetto nome cluster (oggetto nome cluster) non è impostato come protezione BitLocker valida per questo volume. Per risolvere il problema, rimuovere il disco dal cluster. Usare quindi lo strumento da riga di comando Manage-bde. exe per aggiungere il nome del cluster come protezione ADAccountOrGroup, usando il formato Domain\\clustername\$e aggiungere di nuovo il disco al cluster. Per ulteriori informazioni, vedere la documentazione di Manage-bde. exe.

### <a name="event-1608-res_fileserver_leader_failed"></a>Evento 1608: RES_FILESERVER_LEADER_FAILED

Impossibile avviare il file server perché non è stata trovata una dipendenza prevista dalla risorsa ' nome di rete ' oppure non è stata configurata correttamente. Errore = 0x %2.

### <a name="event-1609-res_soda_fileserver_leader_failed"></a>Evento 1609: RES_SODA_FILESERVER_LEADER_FAILED

Impossibile avviare il file server Scale Out perché non è stata trovata la dipendenza prevista dalla risorsa ' nome rete distribuità oppure non è stata configurata correttamente. Errore = 0x %2.

### <a name="event-1632-clusapi_create_mismatched_ou"></a>Evento 1632: CLUSAPI_CREATE_MISMATCHED_OU

Creazione del cluster non riuscita. Impossibile creare l'oggetto nome cluster ' %1' nell'unità organizzativa Active Directory ' %2'. L'oggetto esiste già nell'unità organizzativa ' %3'. Verificare che il percorso del nome distinto specificato e l'oggetto nome cluster siano corretti. Se il percorso del nome distinto non è specificato, verrà utilizzato l'oggetto computer esistente ' %1'.

### <a name="event-1652-service_tcp_connection_failure"></a>Evento 1652: SERVICE_TCP_CONNECTION_FAILURE

Il nodo del cluster ' %1' non è riuscito ad aggiungere il cluster. Impossibile stabilire una connessione TCP per i nodi ' %2'. Verificare la connettività di rete e la configurazione di tutti i firewall di rete.

### <a name="event-1652-service_udp_connection_failure"></a>Evento 1652: SERVICE_UDP_CONNECTION_FAILURE

Il nodo del cluster ' %1' non è riuscito ad aggiungere il cluster. Non è stato possibile stabilire una connessione UDP ai nodi ' %2'. Verificare la connettività di rete e la configurazione di tutti i firewall di rete.

### <a name="event-1652-service_virtual_tcp_connection_failure"></a>Evento 1652: SERVICE_VIRTUAL_TCP_CONNECTION_FAILURE

Il nodo del cluster ' %1' non è riuscito ad aggiungere il cluster. Impossibile stabilire una connessione TCP che utilizza la scheda virtuale del cluster di failover Microsoft per i nodi ' %2'. Verificare la connettività di rete e la configurazione di tutti i firewall di rete.

### <a name="event-1653-service_no_connectivity"></a>Evento 1653: SERVICE_NO_CONNECTIVITY

Il nodo cluster ' %1' non è riuscito ad aggiungere il cluster perché non è in grado di comunicare in rete con altri nodi del cluster. Verificare la connettività di rete e la configurazione di tutti i firewall di rete.

### <a name="event-1654-res_vipaddr_invalid_adaptername"></a>Evento 1654: RES_VIPADDR_INVALID_ADAPTERNAME

La risorsa di indirizzo IP non contiguo ' %1' del cluster non è riuscita a tornare in linea. Impossibile determinare i dati di configurazione per la scheda di rete corrispondente alla scheda di rete ' %2' (codice di errore:' %3'). Verificare che la risorsa indirizzo IP sia configurata con l'indirizzo e le proprietà di rete corretti.

### <a name="event-1655-res_vipaddr_invalid_vsid"></a>Evento 1655: RES_VIPADDR_INVALID_VSID

La risorsa di indirizzo IP non contiguo ' %1' del cluster non è riuscita a tornare in linea. Non è stato possibile determinare i dati di configurazione per la scheda di rete corrispondente all'ID subnet virtuale ' %2' e all'ID dominio di routing ' %3' (codice di errore:' %4'). Verificare che la risorsa indirizzo IP sia configurata con l'indirizzo e le proprietà di rete corretti.

### <a name="event-1656-res_vipaddr_address_create_failed"></a>Evento 1656: RES_VIPADDR_ADDRESS_CREATE_FAILED

Non è stato possibile aggiungere l'indirizzo IP ' %2' per la risorsa ' %1' dell'indirizzo IP non contiguo. codice di errore:' %3'. Verificare la presenza di errori hardware o software correlati alle schede di rete fisiche o virtuali.

### <a name="event-1664-cluster_upgrade_incomplete"></a>Evento 1664: CLUSTER_UPGRADE_INCOMPLETE

L'aggiornamento del livello funzionale del cluster non è riuscito. Verificare che tutti i nodi del cluster siano attualmente in esecuzione e che siano della stessa versione di Windows Server, quindi eseguire di nuovo il cmdlet Update-ClusterFunctionalLevel di Windows PowerShell.

### <a name="event-1676-event_local_node_quarantined"></a>Evento 1676: EVENT_LOCAL_NODE_QUARANTINED

Il nodo cluster locale è stato messo in quarantena da' %1'. Il nodo verrà messo in quarantena fino a' %2', quindi il nodo tenterà automaticamente di aggiungere di nuovo il cluster.
<br><br>Per determinare i problemi relativi a questo nodo, vedere i registri eventi di sistema e dell'applicazione. Quando il problema viene risolto, la quarantena può essere cancellata manualmente per consentire il riaggiunta del nodo al cmdlet di Windows PowerShell "Start-ClusterNode – ClearQuarantine".<br><br>QuarantineType: in quarantena da %1<br>L'ora di quarantena verrà cancellata automaticamente: %2

### <a name="event-1677-event_node_drain_failed"></a>Evento 1677: EVENT_NODE_DRAIN_FAILED

Svuotamento del nodo non riuscito nel nodo cluster %1. <br><br>Fare riferimento ai registri eventi di sistema e dell'applicazione del nodo e ai log del cluster per esaminare la cause dell'errore di svuotamento. Quando il problema viene risolto, è possibile ritentare l'operazione di svuotamento.

### <a name="event-1683-res_netname_computer_account_no_dc"></a>Evento 1683: RES_NETNAME_COMPUTER_ACCOUNT_NO_DC

Il servizio cluster non è riuscito a raggiungere alcun controller di dominio disponibile nel dominio. Questo può influito sulle funzionalità che dipendono dall'autenticazione del nome di rete del cluster.<br><br>Server DC: %1 

#### <a name="guidance"></a>Indicazioni

Verificare che i controller di dominio siano accessibili sulla rete ai nodi del cluster.

### <a name="event-1684-res_netname_computer_object_vco_not_found"></a>Evento 1684: RES_NETNAME_COMPUTER_OBJECT_VCO_NOT_FOUND

La risorsa nome di rete cluster non è riuscita a trovare l'oggetto computer associato in Active Directory. Questo può influito sulle funzionalità che dipendono dall'autenticazione del nome di rete del cluster.<br><br>Nome rete: %1<br>Unità organizzativa: %2

#### <a name="guidance"></a>Indicazioni

Ripristinare l'oggetto computer per il nome di rete dal cestino del Active Directory. In alternativa, offline la risorsa nome di rete del cluster ed eseguire l'azione di ripristino per ricreare l'oggetto computer in Active Directory.

### <a name="event-1685-res_netname_computer_object_cno_not_found"></a>Evento 1685: RES_NETNAME_COMPUTER_OBJECT_CNO_NOT_FOUND

La risorsa nome di rete cluster non è riuscita a trovare l'oggetto computer associato in Active Directory. Questo può influito sulle funzionalità che dipendono dall'autenticazione del nome di rete del cluster.<br><br>Nome rete: %1<br>Unità organizzativa: %2
#### <a name="guidance"></a>Indicazioni

Ripristinare l'oggetto computer per il nome di rete dal cestino del Active Directory.

### <a name="event-1686-res_netname_computer_object_vco_disabled"></a>Evento 1686: RES_NETNAME_COMPUTER_OBJECT_VCO_DISABLED

Risorsa nome di rete cluster: è stato trovato l'oggetto computer associato in Active Directory da disabilitare. Questo può influito sulle funzionalità che dipendono dall'autenticazione del nome di rete del cluster.<br><br>Nome rete: %1<br>Unità organizzativa: %2

#### <a name="guidance"></a>Indicazioni

Abilitare l'oggetto computer per il nome di rete in Active Directory.

### <a name="event-1687-res_netname_computer_object_cno_disabled"></a>Evento 1687: RES_NETNAME_COMPUTER_OBJECT_CNO_DISABLED

Risorsa nome di rete cluster: è stato trovato l'oggetto computer associato in Active Directory da disabilitare. Questo può influito sulle funzionalità che dipendono dall'autenticazione del nome di rete del cluster.<br><br>Nome rete: %1<br>Unità organizzativa: %2

#### <a name="guidance"></a>Indicazioni

Abilitare l'oggetto computer per il nome di rete in Active Directory. In alternativa, offline la risorsa nome di rete del cluster ed eseguire l'azione di ripristino per abilitare l'oggetto computer in Active Directory.

### <a name="event-1688-res_netname_computer_object_failed"></a>Evento 1688: RES_NETNAME_COMPUTER_OBJECT_FAILED

La risorsa nome di rete cluster ha rilevato che l'oggetto computer associato in Active Directory è stato disabilitato e non è riuscito nel tentativo di abilitarlo. Questo può influito sulle funzionalità che dipendono dall'autenticazione del nome di rete del cluster.<br><br>Nome rete: %1<br>Unità organizzativa: %2

#### <a name="guidance"></a>Indicazioni

Abilitare l'oggetto computer per il nome di rete in Active Directory.

### <a name="event-4608-nodecleanup_get_clustered_disks_failed"></a>Evento 4608: NODECLEANUP_GET_CLUSTERED_DISKS_FAILED

Servizio cluster non è riuscito a recuperare l'elenco dei dischi del cluster durante l'eliminazione del cluster. Codice di errore:' %1'. Se questi dischi non sono accessibili, eseguire il cmdlet di PowerShell "Clear-ClusterDiskReservation".

### <a name="event-4611-nodecleanup_release_clustered_disks_from_partmgr_failed"></a>Evento 4611: NODECLEANUP_RELEASE_CLUSTERED_DISKS_FROM_PARTMGR_FAILED

Il disco del cluster con ID ' %2' non è stato rilasciato da Gestione partizioni durante l'eliminazione del cluster. Codice di errore:' %1'. Il riavvio del computer assicurerà che il disco venga rilasciato da Gestione partizioni.

### <a name="event-4613-nodecleanup_clear_clusdisk_database_failed"></a>Evento 4613: NODECLEANUP_CLEAR_CLUSDISK_DATABASE_FAILED

Il servizio cluster non è riuscito a pulire correttamente un disco del cluster con ID ' %2' durante l'eliminazione del cluster. Codice di errore:' %1'. Potrebbe non essere possibile accedere a questo disco fino a quando la pulizia non è stata completata correttamente. Per la pulizia manuale, eliminare il valore "AttachedDisks" della chiave "HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\ClusDisk\\Parameters" nel registro di sistema di Windows.

### <a name="event-4615-nodecleanup_disable_cluster_service_failed"></a>Evento 4615: NODECLEANUP_DISABLE_CLUSTER_SERVICE_FAILED

Il servizio cluster è stato arrestato e impostato come disabilitato come parte della pulizia dei nodi del cluster.

### <a name="event-4616-nodecleanup_disable_cluster_service_timeout"></a>Evento 4616: NODECLEANUP_DISABLE_CLUSTER_SERVICE_TIMEOUT

La chiusura del servizio cluster durante la pulizia del nodo cluster non è stata completata entro il periodo di tempo previsto. Riavviare il computer per verificare che il servizio cluster non sia più in esecuzione.

### <a name="event-4618-nodecleanup_reset_cluster_registry_entries_failed"></a>Evento 4618: NODECLEANUP_RESET_CLUSTER_REGISTRY_ENTRIES_FAILED

Impossibile reimpostare le voci del registro di sistema del servizio cluster durante la pulizia del nodo cluster.
Codice di errore:' %1'. Potrebbe non essere possibile creare o unire in join un cluster con questo computer finché la pulizia non è stata completata correttamente. Per la pulizia manuale, eseguire il cmdlet di PowerShell ' Clear-ClusterNode ' in questo computer.

### <a name="event-4620-nodecleanup_unload_cluster_hive_failed"></a>Evento 4620: NODECLEANUP_UNLOAD_CLUSTER_HIVE_FAILED

Lo scaricamento dell'hive del registro di sistema del servizio cluster durante la pulizia del nodo cluster non è riuscito.
Codice di errore:' %1'. Potrebbe non essere possibile creare o unire in join un cluster con questo computer finché la pulizia non è stata completata correttamente. Per la pulizia manuale, eseguire il cmdlet di PowerShell ' Clear-ClusterNode ' in questo computer.

### <a name="event-4622-nodecleanup_errors"></a>Evento 4622: NODECLEANUP_ERRORS

Il Servizio cluster ha rilevato un errore durante la pulizia del nodo. Potrebbe non essere possibile creare o unire in join un cluster con questo computer finché la pulizia non è stata completata correttamente. Usare il cmdlet di PowerShell ' Clear-ClusterNode ' in questo nodo.

### <a name="event-4624-nodecleanup_reset_nlbsflags_failed"></a>Evento 4624: NODECLEANUP_RESET_NLBSFLAGS_FAILED

Impossibile reimpostare il valore del registro di sistema IPSec Security Association timeout durante la pulizia del nodo del cluster. Codice di errore:' %1'. Per la pulizia manuale, eseguire il cmdlet di PowerShell ' Clear-ClusterNode ' in questo computer. In alternativa, è possibile reimpostare il timeout dell'associazione di sicurezza IPSec eliminando il valore "%2" e il valore "%3" da HKEY_LOCAL_MACHINE nel registro di sistema di Windows.

### <a name="event-4627-nodecleanup_delete_cluster_tasks_failed"></a>Evento 4627: NODECLEANUP_DELETE_CLUSTER_TASKS_FAILED

L'eliminazione delle attività cluster durante la pulizia del nodo non è riuscita. Codice di errore:' %1'.
Utilizzare Utilità di pianificazione Windows per eliminare eventuali attività del cluster rimanenti.

### <a name="event-4629-nodecleanup_delete_local_account_failed"></a>Evento 4629: NODECLEANUP_DELETE_LOCAL_ACCOUNT_FAILED

Durante la pulizia del nodo, l'account utente locale gestito dal cluster non è stato eliminato. Codice di errore:' %1'. Aprire utenti e gruppi locali (lusrmgr. msc) per eliminare l'account.

### <a name="event-4864-res_vsstask_open_failed"></a>Evento 4864: RES_VSSTASK_OPEN_FAILED

Creazione della risorsa attività servizio Copia Shadow del volume ' %1' non riuscita. Codice di errore:' %2'.

### <a name="event-4865-res_vsstask_terminate_task_failed"></a>Evento 4865: RES_VSSTASK_TERMINATE_TASK_FAILED

Risorsa attività del servizio Copia Shadow del volume ' %1' non riuscita. Codice di errore:' %2'.
Ciò è dovuto al fatto che non è stato possibile arrestare l'attività associata come parte di un'operazione offline. Potrebbe essere necessario arrestarlo manualmente usando lo snap-in Utilità di pianificazione.

### <a name="event-4866-res_vsstask_delete_task_failed"></a>Evento 4866: RES_VSSTASK_DELETE_TASK_FAILED

Risorsa attività del servizio Copia Shadow del volume ' %1' non riuscita. Codice di errore:' %2'.
Ciò è dovuto al fatto che l'attività associata non può essere eliminata come parte di un'operazione offline. Potrebbe essere necessario eliminarlo manualmente usando lo snap-in Utilità di pianificazione.

### <a name="event-4867-res_vsstask_online_failed"></a>Evento 4867: RES_VSSTASK_ONLINE_FAILED

Risorsa attività del servizio Copia Shadow del volume ' %1' non riuscita. Codice di errore:' %2'.
Questo perché non è stato possibile aggiungere l'attività associata come parte di un'operazione online. Per assicurarsi che le attività siano configurate correttamente, utilizzare lo snap-in Utilità di pianificazione.

### <a name="event-4868-unable_to_start_autologger"></a>Evento 4868: UNABLE_TO_START_AUTOLOGGER

Servizio cluster non è riuscito ad avviare la sessione di traccia del log del cluster. Codice di errore:' %2'. Il cluster funzionerà correttamente, ma le informazioni di registrazione aggiuntive saranno mancanti. Utilizzare lo snap-in Performance Monitor per verificare le impostazioni del canale eventi per ' %1'.

### <a name="event-4869-netft_watchdog_process_hung"></a>Evento 4869: NETFT_WATCHDOG_PROCESS_HUNG

Il monitoraggio dell'integrità della modalità utente ha rilevato che il sistema non risponde. La scheda virtuale del cluster di failover ha perso il contatto con il processo ' %1' con ID processo ' %2' per ' %3' secondi. Utilizzare Performance Monitor per valutare l'integrità del sistema e determinare quale processo potrebbe avere un impatto negativo sul sistema.

### <a name="event-4870-netft_watchdog_process_terminated"></a>Evento 4870: NETFT_WATCHDOG_PROCESS_TERMINATED

Il monitoraggio dell'integrità della modalità utente ha rilevato che il sistema non risponde. La scheda virtuale del cluster di failover ha perso il contatto con il processo ' %1' con ID processo ' %2' per ' %3' secondi. Verrà eseguita l'azione di ripristino.

### <a name="event-4871-netft_miniport_initialization_failure"></a>Evento 4871: NETFT_MINIPORT_INITIALIZATION_FAILURE

Impossibile avviare il servizio cluster. Perché la scheda miniport del cluster di failover non è riuscita a inizializzare la scheda miniport. Codice di errore:' %1'. Verificare che le altre schede di rete funzionino correttamente e controllare la presenza di errori in gestione dispositivi. Se la configurazione è stata modificata, potrebbe essere necessario reinstallare la funzionalità clustering di failover nel computer.

### <a name="event-4872-netft_missing_datalink_address"></a>Evento 4872: NETFT_MISSING_DATALINK_ADDRESS

La scheda virtuale del cluster di failover non è riuscita a generare un indirizzo MAC univoco.
Non è stato possibile trovare una scheda Ethernet fisica da cui generare un indirizzo univoco o l'indirizzo generato è in conflitto con un altro adapter nel computer. Eseguire la convalida guidata configurazione per verificare la configurazione di rete.

### <a name="event-5122-dcm_event_root_creation_failed"></a>Evento 5122: DCM_EVENT_ROOT_CREATION_FAILED

Servizio cluster non è stato possibile creare la directory radice dei volumi condivisi cluster ' %2'.
Messaggio di errore:' %1'.

### <a name="event-5142-dcm_volume_no_access"></a>Evento 5142: DCM_VOLUME_NO_ACCESS

Il Volume condiviso cluster ' %1' (' %2') non è più accessibile da questo nodo del cluster a causa dell'errore ' %3'. Risolvere i problemi di connettività del nodo al dispositivo di archiviazione e alla connettività di rete.

### <a name="event-5143-dcm_veto_resource_move_due_to_cc"></a>Evento 5143: DCM_VETO_RESOURCE_MOVE_DUE_TO_CC

Lo spostamento del disco (' %2') è bloccato in base allo stato corrente del gestore della cache nel nodo ' %1' per evitare un potenziale deadlock. ' Propone pagine dirty di gestione cache ' è %3 è pagine dirty di gestione cache ' è %4. Lo spostamento è consentito se le pagine dirty di gestione cache sono inferiori al 70% delle "pagine dirty di gestione cache propone" o se "le pagine dirty di gestione cache propone" meno "pagine dirty di gestione cache" sono maggiori di 128000 pagine (informazioni su 500MB se le dimensioni della pagina sono 4096 byte).
Spostamento della risorsa di cui è stato eseguito il veto del cluster per impedire il potenziale deadlock a causa della limitazione delle richieste di gestione cache durante la sospensione dei volumi condivisi del cluster su questo disco.

### <a name="event-5144-dcm_snapshot_diff_area_failure"></a>Evento 5144: DCM_SNAPSHOT_DIFF_AREA_FAILURE

Durante l'aggiunta del disco (' %1') ai volumi condivisi cluster, l'impostazione dell'associazione di area diff snapshot esplicita per il volume (' %2') non è riuscita con errore ' %3'. L'unica associazione di area diff dello snapshot software supportata per i volumi condivisi del cluster è l'autonomia.

### <a name="event-5145-dcm_snapshot_diff_area_delete_failure"></a>Evento 5145: DCM_SNAPSHOT_DIFF_AREA_DELETE_FAILURE

La risorsa disco del cluster ' %1' non è riuscita a eliminare uno snapshot software. Non è stato possibile annullare l'associazione dell'area diff sul volume ' %3' dal volume ' %2'. Questo problema può essere causato da snapshot attivi. Per i volumi condivisi del cluster è necessario che lo snapshot software si trovi nello stesso disco.

### <a name="event-5146-dcm_veto_resource_move_due_to_dismount"></a>Evento 5146: DCM_VETO_RESOURCE_MOVE_DUE_TO_DISMOUNT

Lo spostamento della risorsa Volume condiviso cluster ' %1' è bloccato perché uno dei volumi appartenenti alla risorsa è in stato smontato. Ripetere l'azione dopo il completamento dell'operazione di smontaggio.

### <a name="event-5147-dcm_veto_resource_move_due_to_snapshot"></a>Evento 5147: DCM_VETO_RESOURCE_MOVE_DUE_TO_SNAPSHOT

Lo spostamento della risorsa Volume condiviso cluster ' %1' è bloccato perché uno dei volumi appartenenti alla risorsa è in stato smontato. Ripetere l'azione dopo il completamento dell'operazione di smontaggio.

### <a name="event-5148-dcm_veto_resource_move_due_to_io_mode_change"></a>Evento 5148: DCM_VETO_RESOURCE_MOVE_DUE_TO_IO_MODE_CHANGE

Lo spostamento della risorsa Volume condiviso cluster ' %1' è bloccato perché è in corso un'operazione di modifica della modalità i/o (diretto IO su IO reindirizzato o viceversa) in uno dei volumi appartenenti alla risorsa. Ripetere l'azione dopo il completamento dell'operazione.

### <a name="event-5150-dcm_set_resource_in_failed_state"></a>Evento 5150: DCM_SET_RESOURCE_IN_FAILED_STATE

Risorsa disco fisico del cluster ' %1' non riuscita. Il Volume condiviso cluster è stato impostato come non riuscito con l'errore seguente:' %2'

### <a name="event-5200-cam_cannot_create_cno_token"></a>Evento 5200: CAM_CANNOT_CREATE_CNO_TOKEN

Servizio cluster non è stato possibile creare un token di identità del cluster per i volumi condivisi del cluster. Codice di errore:' %1'. Verificare che il controller di dominio sia accessibile e verificare la presenza di problemi di connettività. Fino a quando non viene ripristinata la connessione al controller di dominio, è possibile che alcune operazioni sul nodo rispetto ai volumi condivisi del cluster abbiano esito negativo.

### <a name="event-5216-csv_sw_snapshot_failed"></a>Evento 5216: CSV_SW_SNAPSHOT_FAILED

Creazione dello snapshot del software su Volume condiviso cluster ' %1' (' %2') non riuscita con errore %3. La risorsa deve essere online per supportare la creazione di snapshot. Verificare lo stato della risorsa.

### <a name="event-5217-csv_sw_snapshot_set_failed"></a>Evento 5217: CSV_SW_SNAPSHOT_SET_FAILED

La creazione di snapshot software nei Volume condiviso cluster (' %1') con ID set di snapshot ' %2' non è riuscita con errore ' %3'. Verificare lo stato delle risorse CSV e gli eventi di sistema dei nodi del proprietario della risorsa.

### <a name="event-5219-csv_register_snapshot_prov_with_vss_failed"></a>Evento 5219: CSV_REGISTER_SNAPSHOT_PROV_WITH_VSS_FAILED

Servizio cluster non è stato possibile registrare il provider di snapshot dei volumi condivisi cluster con il servizio shadow del volume (VSS). Questo problema potrebbe essere dovuto all'arresto del servizio VSS o a un problema con il servizio VSS che causa un problema che non accetta le richieste in ingresso. <br>Errore: %1

### <a name="event-5377-operation_exceeded_timeout"></a>Evento 5377: OPERATION_EXCEEDED_TIMEOUT

Un'operazione di Servizio cluster interna ha superato la soglia specificata di ' %2' secondi. Il Servizio cluster è stato terminato per il ripristino. Gestione controllo servizi riavvierà il Servizio cluster e il nodo si riunirà al cluster.

### <a name="event-5396-two_partitions_have_quorum"></a>Evento 5396: TWO_PARTITIONS_HAVE_QUORUM

Il Servizio cluster in questo nodo viene arrestato perché è stato rilevato che sono presenti altri nodi del cluster con quorum. Questo errore si verifica quando il Servizio cluster rileva un altro nodo avviato con l'opzione Force quorum switch (/FQ). Il nodo avviato con l'opzione di quorum forzato rimarrà in esecuzione. Usare Gestione cluster di failover per verificare che il nodo venga aggiunto automaticamente al cluster al riavvio del servizio cluster.

### <a name="event-5397-rlua_account_failed"></a>Evento 5397: RLUA_ACCOUNT_FAILED

La risorsa cluster ' %1' non è in grado di creare o modificare l'account utente locale replicato ' %2' in questo nodo. Per ulteriori informazioni, controllare i log del cluster.

### <a name="event-5398-nm_event_cluster_failed_to_form"></a>Evento 5398: NM_EVENT_CLUSTER_FAILED_TO_FORM

Non è stato possibile avviare il cluster. La copia più recente dei dati di configurazione del cluster non è disponibile nel set di nodi che tentano di avviare il cluster. Le modifiche apportate al cluster si sono verificate quando il set di nodi non è membro e di conseguenza non è stato possibile ricevere gli aggiornamenti dei dati di configurazione. .<br><br>Voti necessari per avviare il cluster: %1<br>Voti disponibili: %2<br>Nodi con voti: %3

#### <a name="guidance"></a>Indicazioni

Provare ad avviare il servizio cluster in tutti i nodi del cluster in modo che i nodi con la copia più recente dei dati di configurazione del cluster possano prima formare il cluster. Il cluster sarà in grado di avviarsi e i nodi otterranno automaticamente i dati di configurazione del cluster aggiornati. Se non sono disponibili nodi con la copia più recente dei dati di configurazione del cluster, eseguire il cmdlet di Windows PowerShell ' Start-ClusterNode-FQ '. Se si usa il parametro ForceQuorum (FQ), il servizio cluster verrà avviato e la copia del nodo dei dati di configurazione del cluster verrà contrassegnata come autorevole. La forzatura del quorum in un nodo con una copia obsoleta del database cluster può comportare modifiche della configurazione del cluster che si sono verificate durante la mancata partecipazione del nodo al cluster.

## <a name="warning-events"></a>Eventi di avviso

### <a name="event-1011-nm_node_evicted"></a>Evento 1011: NM_NODE_EVICTED

Il nodo del cluster %1 è stato eliminato dal cluster di failover.

### <a name="event-1045-res_ipaddr_ipv4_address_create_failed"></a>Evento 1045: RES_IPADDR_IPV4_ADDRESS_CREATE_FAILED

Non sono state trovate interfacce di rete corrispondenti per l'indirizzo IP ' %2' della risorsa ' %1'. codice restituito:' %3'. Questo può essere normale se i nodi del cluster si estendono su subnet diverse.

### <a name="event-1066-res_disk_corrupt_disk"></a>Evento 1066: RES_DISK_CORRUPT_DISK

La risorsa disco del cluster ' %1' indica il danneggiamento del volume ' %2'. CHKDSK viene eseguito per risolvere i problemi. Il disco non sarà disponibile fino al completamento di CHKDSK.
L'output CHKDSK verrà registrato nel file ' %3'.<br> CHKDSK può inoltre scrivere informazioni nel registro eventi dell'applicazione.

### <a name="event-1068-res_smb_share_cant_add"></a>Evento 1068: RES_SMB_SHARE_CANT_ADD

Impossibile portare in linea la risorsa di condivisione file del cluster ' %1'. La creazione della condivisione file ' %2' (con ambito al nome di rete %3) non è riuscita a causa dell'errore ' %4'. Questa operazione verrà ritentata automaticamente.

### <a name="event-1071-rcm_resource_online_blocked_by_locked_mode"></a>Evento 1071: RCM_RESOURCE_ONLINE_BLOCKED_BY_LOCKED_MODE

Il tentativo di eseguire l'operazione sulla risorsa cluster ' %1' di tipo ' %3' nel ruolo del cluster ' %2' non è stato completato perché la risorsa o uno dei relativi provider ha bloccato lo stato.

### <a name="event-1071-rcm_resource_offline_blocked_by_locked_mode"></a>Evento 1071: RCM_RESOURCE_OFFLINE_BLOCKED_BY_LOCKED_MODE

Il tentativo di eseguire l'operazione sulla risorsa cluster ' %1' di tipo ' %3' nel ruolo del cluster ' %2' non è stato completato perché la risorsa o uno dei suoi dipendenti ha bloccato lo stato.

### <a name="event-1094-sm_invalid_security_level"></a>Evento 1094: SM_INVALID_SECURITY_LEVEL

Impossibile modificare la proprietà comune del cluster SecurityLevel in questo cluster. Non è possibile modificare il livello di sicurezza del cluster perché il cluster è attualmente configurato per nessuna modalità di autenticazione.

### <a name="event-1119-res_netname_dns_registration_missing"></a>Evento 1119: RES_NETNAME_DNS_REGISTRATION_MISSING

La risorsa del nome di rete del cluster ' %1' non è riuscita a registrare il nome DNS ' %2' sulla scheda ' %4' per il motivo seguente: <br><br>' %3'

### <a name="event-1125-tm_event_cluster_netinterface_unreachable"></a>Evento 1125: TM_EVENT_CLUSTER_NETINTERFACE_UNREACHABLE

L'interfaccia di rete del cluster ' %1' per il nodo cluster ' %2' nella rete ' %3' non è raggiungibile da almeno un altro nodo del cluster collegato alla rete. Il cluster di failover non è stato in grado di determinare il percorso dell'errore. Eseguire la convalida guidata configurazione per verificare la configurazione di rete. Se la condizione è permanente, verificare la presenza di errori hardware o software correlati alla scheda di rete. Controllare anche la presenza di errori in qualsiasi altro componente di rete a cui è connesso il nodo, ad esempio hub, commutatori o Bridge.

### <a name="event-1149-res_netname_cant_delete_dns_records"></a>Evento 1149: RES_NETNAME_CANT_DELETE_DNS_RECORDS

I record host DNS (A) e puntatore (PTR) associati alla risorsa cluster ' %1' non sono stati rimossi dal server DNS associato della risorsa. Se necessario, è possibile eliminarli manualmente. Contattare l'amministratore DNS per contribuire a questo sforzo.

### <a name="event-1150-res_netname_dns_ptr_record_delete_failed"></a>Evento 1150: RES_NETNAME_DNS_PTR_RECORD_DELETE_FAILED

Rimozione del record puntatore DNS (PTR)' %2' per l'host ' %3' associata alla risorsa nome di rete cluster ' %1' non riuscita. errore:' %4'.
Se necessario, il record può essere eliminato manualmente. Per assistenza, contattare l'amministratore DNS.

### <a name="event-1151-res_netname_dns_a_record_delete_failed"></a>Evento 1151: RES_NETNAME_DNS_A_RECORD_DELETE_FAILED

La rimozione del record host DNS (A)' %2' associata alla risorsa nome di rete cluster ' %1' non è riuscita con errore ' %3'. Se necessario, il record può essere eliminato manualmente. Per assistenza, contattare l'amministratore DNS.

### <a name="event-1155-rcm_event_exited_queuing"></a>Evento 1155: RCM_EVENT_EXITED_QUEUING

Lo spostamento in sospeso per il ruolo ' %1' non è stato completato.

### <a name="event-1197-res_netname_delete_disable_failed"></a>Evento 1197: RES_NETNAME_DELETE_DISABLE_FAILED

La risorsa del nome di rete del cluster ' %1' non è riuscita a eliminare o disabilitare l'oggetto computer associato ' %2' durante l'eliminazione della risorsa. Codice di errore:' %3'. <br>Verificare se il sito è di sola lettura. Verificare che l'oggetto nome cluster disponga delle autorizzazioni appropriate per l'oggetto ' %2' in Active Directory.

### <a name="event-1198-res_netname_delete_vco_guid_failed"></a>Evento 1198: RES_NETNAME_DELETE_VCO_GUID_FAILED

La risorsa del nome di rete del cluster ' %1' non è riuscita a eliminare l'oggetto computer con GUID ' %2'. Codice di errore:' %3'. <br>Verificare se il sito è di sola lettura. Verificare che l'oggetto nome cluster disponga delle autorizzazioni appropriate per l'oggetto ' %2' in Active Directory.

### <a name="event-1216-service_netname_change_warning"></a>Evento 1216: SERVICE_NETNAME_CHANGE_WARNING

L'operazione di modifica del nome nella risorsa NetName core del cluster non è riuscita.
Anche il tentativo di ripristinare il nome originale dell'operazione di modifica del nome non è riuscito. Codice di errore:' %1'. Potrebbe non essere possibile gestire in remoto il cluster usando il nome del cluster fino a quando questa situazione non è stata corretta manualmente.

### <a name="event-1221-res_netname_rename_out_of_synch_with_compobj"></a>Evento 1221: RES_NETNAME_RENAME_OUT_OF_SYNCH_WITH_COMPOBJ

La risorsa nome di rete cluster ' %1' ha un nome ' %2' che non corrisponde al nome dell'oggetto computer corrispondente ' %3'. È probabile che una modifica del nome precedente dell'oggetto computer non sia stata replicata in tutti i controller di dominio del dominio. Non sarà possibile rinominare la risorsa del nome di rete finché i nomi non diventeranno coerenti. Se l'oggetto computer non è stato modificato di recente, contattare l'amministratore di dominio per rinominare l'oggetto computer e renderlo quindi coerente. Inoltre, assicurarsi che la replica tra i controller di dominio sia stata completata correttamente.

### <a name="event-1222-res_netname_set_permissions_failed"></a>Evento 1222: RES_NETNAME_SET_PERMISSIONS_FAILED

Impossibile aggiornare l'oggetto computer associato alla risorsa nome di rete cluster ' %1'.<br><br>Il testo per il codice di errore associato è: %2<br> <br>L'identità del cluster ' %3' potrebbe non avere le autorizzazioni necessarie per aggiornare l'oggetto. Collaborare con l'amministratore di dominio per assicurarsi che l'identità del cluster possa aggiornare gli oggetti computer nel dominio.

### <a name="event-1240-res_ipaddr_obtain_lease_failed"></a>Evento 1240: RES_IPADDR_OBTAIN_LEASE_FAILED

La risorsa indirizzo IP del cluster ' %1' non è riuscita a ottenere un indirizzo IP con lease.

### <a name="event-1243-res_ipaddr_release_lease_failed"></a>Evento 1243: RES_IPADDR_RELEASE_LEASE_FAILED

La risorsa indirizzo IP del cluster ' %1' non è riuscita a rilasciare il lease per l'indirizzo IP ' %2'.

### <a name="event-1251-rcm_group_preempted"></a>Evento 1251: RCM_GROUP_PREEMPTED

Il ruolo del cluster ' %2' è stato portato offline. Questo ruolo è stato interrotto dal ruolo del cluster con priorità più alta ' %1'. Il servizio cluster tenterà di portare online il ruolo del cluster ' %2' in un secondo momento quando sono disponibili risorse di sistema.

### <a name="event-1544-service_vss_onabort"></a>Evento 1544: SERVICE_VSS_ONABORT

L'operazione di backup per i dati di configurazione del cluster è stata annullata. Il writer del cluster Servizio Copia Shadow del volume (VSS) ha ricevuto una richiesta di interruzione.

### <a name="event-1548-service_connect_version_compatible"></a>Evento 1548: SERVICE_CONNECT_VERSION_COMPATIBLE

Il nodo ' %1' ha stabilito la comunicazione con il nodo ' %2' e ha rilevato che è in esecuzione una versione diversa, ma compatibile, del sistema operativo. È consigliabile che tutti i nodi eseguano la stessa versione del sistema operativo. Dopo l'aggiornamento di tutti i nodi, eseguire il cmdlet Update-ClusterFunctionalLevel di Windows PowerShell per completare l'aggiornamento del cluster.

### <a name="event-1550-service_connect_novercheck"></a>Evento 1550: SERVICE_CONNECT_NOVERCHECK

Il nodo ' %1' ha stabilito una sessione di comunicazione con nodo ' %2' senza eseguire una verifica di compatibilità della versione perché la verifica della compatibilità della versione è disabilitata. La disabilitazione del controllo della compatibilità delle versioni non è supportata.

### <a name="event-1551-service_form_novercheck"></a>Evento 1551: SERVICE_FORM_NOVERCHECK

Il nodo ' %1' ha formato un cluster di failover senza eseguire una verifica di compatibilità della versione perché la verifica della compatibilità della versione è disabilitata. La disabilitazione del controllo della compatibilità delle versioni non è supportata.

### <a name="event-1555-service_netft_disable_autoconfig_failed"></a>Evento 1555: SERVICE_NETFT_DISABLE_AUTOCONFIG_FAILED

Il tentativo di utilizzare IPv4 per la scheda di rete ' %1' non è riuscito. Ciò è dovuto a un errore di disabilitazione della configurazione automatica IPv4 e del DHCP. Questa operazione può essere prevista se il servizio client DHCP è già disabilitato. IPv6 verrà utilizzato se abilitato; in caso contrario, il nodo potrebbe non essere in grado di partecipare a un cluster di failover.

### <a name="event-1558-service_witness_failover_attempt"></a>Evento 1558: SERVICE_WITNESS_FAILOVER_ATTEMPT

Il servizio cluster ha rilevato un problema con la risorsa del server di controllo del mirroring. Verrà eseguito il failover della risorsa di controllo in un altro nodo all'interno del cluster nel tentativo di ristabilire l'accesso ai dati di configurazione del cluster.

### <a name="event-1562-res_fsw_alivefailure"></a>Evento 1562: RES_FSW_ALIVEFAILURE

La risorsa di controllo di condivisione file ' %1' non ha superato un controllo di integrità periodico nella condivisione file ' %2'. Assicurarsi che la condivisione file ' %2' esista e che sia accessibile dal cluster.

### <a name="event-1576-res_netname_dns_registration_secure_zone_refused"></a>Evento 1576: RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_REFUSED

La risorsa del nome di rete del cluster ' %1' non è riuscita a registrare il nome ' %2' sulla scheda ' %4' in una zona DNS protetta. Questo è dovuto a un record esistente con questo nome e l'identità del cluster non dispone dei privilegi sufficienti per aggiornare il record. Codice di errore:' %3'. Contattare l'amministratore del server DNS per verificare che l'identità del cluster disponga delle autorizzazioni per il record DNS ' %2'.

### <a name="event-1576-res_netname_dns_registration_secure_zone_refused"></a>Evento 1576: RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_REFUSED

La risorsa del nome di rete del cluster ' %1' non è riuscita a registrare il nome ' %2' sulla scheda ' %4' in una zona DNS protetta. Questo è dovuto a un record esistente con questo nome e l'identità del cluster non dispone dei privilegi sufficienti per aggiornare il record. Codice di errore:' %3'. Contattare l'amministratore del server DNS per verificare che l'identità del cluster disponga delle autorizzazioni per il record DNS ' %2'.

### <a name="event-1577-res_netname_dns_server_could_not_be_contacted"></a>Evento 1577: RES_NETNAME_DNS_SERVER_COULD_NOT_BE_CONTACTED

La risorsa del nome di rete del cluster ' %1' non è riuscita a registrare il nome ' %2' sulla scheda ' %4'. Impossibile contattare il server DNS. Codice di errore:' %3'. Verificare che un server DNS sia accessibile da questo nodo del cluster. La registrazione DNS verrà ritentata in un secondo momento.

### <a name="event-1577-res_netname_dns_server_could_not_be_contacted"></a>Evento 1577: RES_NETNAME_DNS_SERVER_COULD_NOT_BE_CONTACTED

La risorsa del nome di rete del cluster ' %1' non è riuscita a registrare il nome ' %2' sulla scheda ' %4'. Impossibile contattare il server DNS. Codice di errore:' %3'. Verificare che un server DNS sia accessibile da questo nodo del cluster. La registrazione DNS verrà ritentata in un secondo momento.

### <a name="event-1578-res_netname_dns_test_for_dynamic_update_failed"></a>Evento 1578: RES_NETNAME_DNS_TEST_FOR_DYNAMIC_UPDATE_FAILED

La risorsa del nome di rete del cluster ' %1' non è riuscita a registrare gli aggiornamenti dinamici per il nome ' %2' sulla scheda ' %4'. Il server DNS potrebbe non essere configurato per accettare gli aggiornamenti dinamici. Codice di errore:' %3'. Contattare l'amministratore del server DNS per verificare che il server DNS sia disponibile e configurato per gli aggiornamenti dinamici.<br><br>In alternativa, è possibile disabilitare gli aggiornamenti DNS dinamici deselezionando l'impostazione "registra gli indirizzi della connessione in DNS" nelle impostazioni avanzate TCP/IP per l'adapter "%4" nella scheda DNS.

### <a name="event-1578-res_netname_dns_test_for_dynamic_update_failed"></a>Evento 1578: RES_NETNAME_DNS_TEST_FOR_DYNAMIC_UPDATE_FAILED

La risorsa del nome di rete del cluster ' %1' non è riuscita a registrare gli aggiornamenti dinamici per il nome ' %2' sulla scheda ' %4'. Il server DNS potrebbe non essere configurato per accettare gli aggiornamenti dinamici. Codice di errore:' %3'. Contattare l'amministratore del server DNS per verificare che il server DNS sia disponibile e configurato per gli aggiornamenti dinamici.<br><br>In alternativa, è possibile disabilitare gli aggiornamenti DNS dinamici deselezionando l'impostazione "registra gli indirizzi della connessione in DNS" nelle impostazioni avanzate TCP/IP per l'adapter "%4" nella scheda DNS.

### <a name="event-1579-res_netname_dns_record_update_failed"></a>Evento 1579: RES_NETNAME_DNS_RECORD_UPDATE_FAILED

La risorsa del nome di rete del cluster ' %1' non è riuscita ad aggiornare il record DNS per il nome ' %2' sulla scheda ' %4'. Codice di errore:' %3'. Verificare che un server DNS sia accessibile da questo nodo del cluster e contattare l'amministratore del server DNS per verificare che l'identità del cluster sia in grado di aggiornare il record DNS ' %2'.

### <a name="event-1579-res_netname_dns_record_update_failed"></a>Evento 1579: RES_NETNAME_DNS_RECORD_UPDATE_FAILED

La risorsa del nome di rete del cluster ' %1' non è riuscita ad aggiornare il record DNS per il nome ' %2' sulla scheda ' %4'. Codice di errore:' %3'. Verificare che un server DNS sia accessibile da questo nodo del cluster e contattare l'amministratore del server DNS per verificare che l'identità del cluster sia in grado di aggiornare il record DNS ' %2'.

### <a name="event-1581-clussvc_unable_to_move_hive_to_safe_file"></a>Evento 1581: CLUSSVC_UNABLE_TO_MOVE_HIVE_TO_SAFE_FILE

La richiesta di ripristino per i dati di configurazione del cluster non è riuscita a creare una copia del file di dati di configurazione del cluster esistente (ClusDB). Durante il tentativo di mantenere la configurazione esistente, l'operazione di ripristino non è riuscita a creare una copia nel percorso ' %1'. Questa situazione potrebbe essere prevista se il file di dati di configurazione esistente è danneggiato. L'operazione di ripristino è stata continuata, ma i tentativi di ripristinare la configurazione del cluster esistente potrebbero non essere possibili.

### <a name="event-1582-clussvc_unable_to_move_restored_hive_to_current"></a>Evento 1582: CLUSSVC_UNABLE_TO_MOVE_RESTORED_HIVE_TO_CURRENT

Il servizio cluster non è riuscito a spostare l'hive del cluster ripristinato in ' %1' in ' %2'. Questo potrebbe impedire che l'operazione di ripristino venga completata correttamente. Se la configurazione del cluster non è stata ripristinata correttamente, ripetere l'operazione di ripristino.

### <a name="event-1583-clussvc_netft_disable_connectionsecurity_failed"></a>Evento 1583: CLUSSVC_NETFT_DISABLE_CONNECTIONSECURITY_FAILED

Servizio cluster non è stato possibile disabilitare Internet Protocol Security (IPsec) nella scheda virtuale del cluster di failover ' %1'. Questo potrebbe avere un impatto negativo sulle prestazioni di comunicazione del cluster. Se il problema persiste, verificare i criteri di sicurezza delle connessioni locali e di dominio che si applicano a IPSec e al Windows Firewall. Verificare inoltre la presenza di eventi correlati al servizio motore di filtro di base.

### <a name="event-1584-shared_volume_not_ready_for_snapshot"></a>Evento 1584: SHARED_VOLUME_NOT_READY_FOR_SNAPSHOT

Un'applicazione di backup ha avviato uno snapshot VSS sul Volume condiviso cluster ' %1' (' %3') senza preparare correttamente il volume per lo snapshot. Questo snapshot potrebbe non essere valido e il backup potrebbe non essere utilizzabile per le operazioni di ripristino. Contattare il fornitore dell'applicazione di backup per verificare la compatibilità con i volumi condivisi del cluster.

### <a name="event-1589-res_netname_dns_returning_ip_that_is_not_provider"></a>Evento 1589: RES_NETNAME_DNS_RETURNING_IP_THAT_IS_NOT_PROVIDER

La risorsa del nome di rete del cluster ' %1' ha rilevato uno o più indirizzi IP associati al nome DNS ' %2' che non sono risorse di indirizzo IP dipendenti. Sono stati trovati indirizzi aggiuntivi "%3". Questa operazione può influire sulla connettività client fino a quando il nome di rete e i record DNS associati non sono coerenti. Contattare l'amministratore del server DNS per verificare i record associati al nome ' %2'.

### <a name="event-1604-res_disk_chkdsk_spotfix_needed"></a>Evento 1604: RES_DISK_CHKDSK_SPOTFIX_NEEDED

La risorsa disco del cluster ' %1' ha rilevato un danneggiamento per il volume ' %2'. Spotfix CHKDSK è necessario per risolvere i problemi.

### <a name="event-1605-res_disk_spotfix_performed"></a>Evento 1605: RES_DISK_SPOTFIX_PERFORMED

La risorsa disco del cluster ' %1' ha completato l'esecuzione di ChkDsk. exe/spotfix nel volume ' %2'.
Codice restituito:' %4'. L'output di ChkDsk è stato registrato nel file ' %3'.<br>
Per ulteriori informazioni da ChkDsk, consultare il registro eventi dell'applicazione.

### <a name="event-1671-res_disk_online_set_attributes_completed_failure"></a>Evento 1671: RES_DISK_ONLINE_SET_ATTRIBUTES_COMPLETED_FAILURE

Impossibile portare in linea la risorsa disco fisico del cluster.<br><br>Nome risorsa disco fisico: %1<br>Codice errore: %2<br>Tempo trascorso (secondi): %3

#### <a name="guidance"></a>Indicazioni

Eseguire la convalida guidata configurazione per verificare la configurazione dell'archiviazione. Se il codice di errore è stato ERROR_CLUSTER_SHUTDOWN, lo stato online in sospeso è stato annullato da un amministratore. Se si tratta di un volume replicato, questo potrebbe essere il risultato di un errore di impostazione degli attributi del disco. Per ulteriori informazioni, esaminare gli eventi di replica di archiviazione.

### <a name="event-1673-cluster_node_entered_isolated_state"></a>Evento 1673: CLUSTER_NODE_ENTERED_ISOLATED_STATE

Il nodo del cluster ' %1' è entrato nello stato isolato.

### <a name="event-1675-event_joining_node_quarantined"></a>Evento 1675: EVENT_JOINING_NODE_QUARANTINED

Il nodo cluster ' %1' è stato messo in quarantena da' %2' e non può essere aggiunto al cluster. Il nodo verrà messo in quarantena fino a' %3', quindi il nodo tenterà automaticamente di aggiungere di nuovo il cluster. <br><br>Per determinare i problemi relativi a questo nodo, vedere i registri eventi di sistema e dell'applicazione. Quando il problema viene risolto, la quarantena può essere cancellata manualmente per consentire il riaggiunta del nodo al cmdlet di Windows PowerShell "Start-ClusterNode – ClearQuarantine".<br><br>Nome nodo: %1<br>QuarantineType: quarantena per %2<br>L'ora di quarantena verrà cancellata automaticamente: %3

### <a name="event-1681-cluster_groups_unmonitored_on_node"></a>Evento 1681: CLUSTER_GROUPS_UNMONITORED_ON_NODE

Le macchine virtuali nel nodo ' %1' hanno acceduto a uno stato non monitorato. Lo stato di integrità delle macchine virtuali verrà nuovamente monitorato quando il nodo torna da uno stato isolato o può eseguire il failover se il nodo non restituisce. La macchina virtuale non è più monitorata: %2.

### <a name="event-1689-event_disable_and_stop_other_service"></a>Evento 1689: EVENT_DISABLE_AND_STOP_OTHER_SERVICE

Il servizio cluster ha rilevato un servizio che non è compatibile con il clustering di failover. Il servizio è stato disabilitato per evitare possibili problemi con il cluster di failover.<br><br>Nome del servizio:' %1'

### <a name="event-4625-nodecleanup_reset_nlbsflags_preserved"></a>Evento 4625: NODECLEANUP_RESET_NLBSFLAGS_PRESERVED

Impossibile reimpostare il valore del registro di sistema IPSec Security Association timeout durante la pulizia del nodo del cluster. Questo è dovuto al fatto che il timeout dell'associazione di sicurezza IPSec è stato modificato dopo la configurazione di questo computer come membro di un cluster. Per la pulizia manuale, eseguire il cmdlet di PowerShell ' Clear-ClusterNode ' in questo computer. In alternativa, è possibile reimpostare il timeout dell'associazione di sicurezza IPSec eliminando il valore "%1" e il valore "%2" da HKEY_LOCAL_MACHINE nel registro di sistema di Windows.

### <a name="event-4873-netft_clussvc_terminate_because_of_watchdog"></a>Evento 4873: NETFT_CLUSSVC_TERMINATE_BECAUSE_OF_WATCHDOG

Il servizio cluster ha rilevato che la scheda virtuale del cluster di failover è stata arrestata. Questo comportamento è previsto quando viene eseguita la CPU a caldo sostituzione in questo nodo.
Servizio cluster verrà arrestato e dovrebbe essere riavviato automaticamente al termine dell'operazione. Verificare la presenza di altri eventi associati al servizio e verificare che il servizio cluster sia stato riavviato in questo nodo.

### <a name="event-5120-dcm_volume_auto_pause_after_failure"></a>Evento 5120: DCM_VOLUME_AUTO_PAUSE_AFTER_FAILURE

Il Volume condiviso cluster ' %1' (' %2') è entrato in uno stato di sospensione a causa di ' %3'.
Tutte le I/O verranno temporaneamente accodate fino a quando non viene ristabilito un percorso del volume.

### <a name="event-5123-dcm_event_root_rename_success"></a>Evento 5123: DCM_EVENT_ROOT_RENAME_SUCCESS

La directory radice dei volumi condivisi del cluster ' %1' esiste già. La directory ' %1' è stata rinominata in ' %2'. Verificare che le applicazioni che accedono ai dati in questo percorso siano state aggiornate secondo necessità.

### <a name="event-5124-dcm_unsafe_filters_found"></a>Evento 5124: DCM_UNSAFE_FILTERS_FOUND

Volume condiviso cluster "%1" ("%3") ha identificato uno o più driver di filtro attivi nello stack del dispositivo che potrebbero interferire con le operazioni CSV. L'accesso i/O verrà reindirizzato al dispositivo di archiviazione attraverso la rete tramite un altro nodo del cluster. Ciò può comportare un calo delle prestazioni. Contattare il fornitore del driver di filtro per verificare l'interoperabilità con volumi condivisi cluster. <br><br>Driver filtro attivi trovati:<br>%2

### <a name="event-5125-dcm_unsafe_volfilter_found"></a>Evento 5125: DCM_UNSAFE_VOLFILTER_FOUND

Volume condiviso cluster "%1" ("%3") ha identificato uno o più driver di volume attivi nello stack del dispositivo che potrebbero interferire con le operazioni CSV. L'accesso i/O verrà reindirizzato al dispositivo di archiviazione attraverso la rete tramite un altro nodo del cluster. Ciò può comportare un calo delle prestazioni. Contattare il fornitore del driver del volume per verificare l'interoperabilità con volumi condivisi cluster. <br><br>Driver del volume attivi trovati:<br>%2

### <a name="event-5126-dcm_event_cannot_disable_short_names"></a>Evento 5126: DCM_EVENT_CANNOT_DISABLE_SHORT_NAMES

La risorsa disco fisico ' %1' non consente la disabilitazione della generazione di nomi brevi. Questo potrebbe causare problemi di compatibilità delle applicazioni. Usare "fsutil 8dot3name set 2" per consentire la disabilitazione della generazione di nomi brevi, quindi offline/online la risorsa.

### <a name="event-5128-dcm_event_cannot_disable_short_names"></a>Evento 5128: DCM_EVENT_CANNOT_DISABLE_SHORT_NAMES

La risorsa disco fisico ' %1' non consente la disabilitazione della generazione di nomi brevi. Questo potrebbe causare problemi di compatibilità delle applicazioni. Usare "fsutil 8dot3name set 2" per consentire la disabilitazione della generazione di nomi brevi, quindi offline/online la risorsa.

### <a name="event-5133-dcm_cannot_restore_drive_letters"></a>Evento 5133: DCM_CANNOT_RESTORE_DRIVE_LETTERS

Il disco del cluster ' %1' è stato rimosso e inserito di nuovo nel gruppo cluster ' available storage '. Durante questo processo, un tentativo di ripristinare le lettere di unità originali ha richiesto più tempo del previsto, probabilmente a causa delle lettere di unità già in uso.

### <a name="event-5134-dcm_cannot_set_acl_on_root"></a>Evento 5134: DCM_CANNOT_SET_ACL_ON_ROOT

Servizio cluster non è stato possibile impostare le autorizzazioni (ACL) nella directory radice dei volumi condivisi del cluster ' %1'. Errore:' %2'.

### <a name="event-5135-dcm_cannot_set_acl_on_volume_folder"></a>Evento 5135: DCM_CANNOT_SET_ACL_ON_VOLUME_FOLDER

Servizio cluster non è stato possibile impostare le autorizzazioni per la directory Volume condiviso cluster ' %1' (' %2'). Errore:' %3'.

### <a name="event-5136-dcm_csv_into_redirected_mode"></a>Evento 5136: DCM_CSV_INTO_REDIRECTED_MODE

È stato attivato l'accesso reindirizzato Volume condiviso cluster ' %1' (' %2'). L'accesso al dispositivo di archiviazione verrà reindirizzato attraverso la rete da tutti i nodi del cluster che accedono a questo volume. Ciò può comportare un calo delle prestazioni. Disabilitare l'accesso reindirizzato per questo volume per riprendere le normali operazioni.

### <a name="event-5149-dcm_csv_block_cache_resized"></a>Evento 5149: DCM_CSV_BLOCK_CACHE_RESIZED

La dimensione della cache è stata ridimensionata a' %1' in base alla memoria popolata, la dimensione dell'utente è troppo elevata.

### <a name="event-5156-dcm_volume_auto_pause_after_snapshot_failure"></a>Evento 5156: DCM_VOLUME_AUTO_PAUSE_AFTER_SNAPSHOT_FAILURE

Il Volume condiviso cluster ' %1' (' %2') è entrato in uno stato di sospensione a causa di ' %3'.
Questo errore si verifica quando gli snapshot VolSnap sottostanti al volume CSV vengono eliminati al di fuori di una richiesta dell'utente. Le possibili cause degli snapshot da eliminare sono la mancanza di spazio nel volume per la crescita degli snapshot o l'errore di i/o durante il tentativo di aggiornare i dati dello snapshot. Tutte le I/O verranno temporaneamente accodate fino a quando lo stato dello snapshot non viene sincronizzato con Volsnap.

### <a name="event-5157-dcm_volume_auto_pause_after_failure_expected"></a>Evento 5157: DCM_VOLUME_AUTO_PAUSE_AFTER_FAILURE_EXPECTED

Il Volume condiviso cluster ' %1' (' %2') è entrato in uno stato di sospensione a causa di ' %3'.
Tutte le I/O verranno temporaneamente accodate fino a quando non viene ristabilito un percorso del volume.
Questo errore è in genere causato da un errore di infrastruttura. Ad esempio, se si perde la connettività all'archiviazione o il nodo proprietario del Volume condiviso cluster rimosso dall'appartenenza al cluster attivo.

### <a name="event-5394-pool_online_warnings"></a>Evento 5394: POOL_ONLINE_WARNINGS

Il Servizio cluster ha rilevato alcuni errori di archiviazione durante il tentativo di portare online il pool di archiviazione. Eseguire la convalida dell'archiviazione del cluster per risolvere il problema.

### <a name="event-5395-rcm_group_auto_move_storage_pool"></a>Evento 5395: RCM_GROUP_AUTO_MOVE_STORAGE_POOL

Il cluster sta migrando il gruppo per il pool di archiviazione ' %1' perché il nodo corrente ' %3' non ha una connettività ottimale ai dischi fisici del pool di archiviazione.

## <a name="informational-events"></a>Eventi informativi

### <a name="event-1592-clussvc_tcp_reconnect_connection_reestablished"></a>Evento 1592: CLUSSVC_TCP_RECONNECT_CONNECTION_REESTABLISHED

Il nodo cluster ' %1' ha perso la comunicazione con il nodo cluster ' %2'. La comunicazione di rete è stata ristabilita. Questo problema potrebbe essere dovuto alla comunicazione temporaneamente bloccata da un firewall o da un aggiornamento dei criteri di sicurezza della connessione. Se il problema persiste e la comunicazione di rete non viene ristabilita, il servizio cluster in uno o più nodi verrà interrotto. In tal caso, eseguire la convalida guidata configurazione per verificare la configurazione di rete. Inoltre, verificare la presenza di errori hardware o software relativi alle schede di rete in questo nodo e verificare la presenza di errori in altri componenti di rete a cui è connesso il nodo, ad esempio hub, commutatori o Bridge.

### <a name="event-1594-cluster_functional_level_upgrade_complete"></a>Evento 1594: CLUSTER_FUNCTIONAL_LEVEL_UPGRADE_COMPLETE

Servizio cluster stato aggiornato correttamente il livello di funzionalità del cluster. <br><br>
Livello di funzionalità: %1 <br> Versione aggiornamento: %2

### <a name="event-1635-rcm_resource_failure_info_with_typename"></a>Evento 1635: RCM_RESOURCE_FAILURE_INFO_WITH_TYPENAME

Risorsa cluster ' %1' di tipo ' %3' nel ruolo del cluster ' %2' non riuscita.

### <a name="event-1636-clussvc_password_changed"></a>Evento 1636: CLUSSVC_PASSWORD_CHANGED

Il Servizio cluster ha modificato la password dell'account ' %1' nel nodo ' %2'.

### <a name="event-1680-cluster_node_exited_isolated_state"></a>Evento 1680: CLUSTER_NODE_EXITED_ISOLATED_STATE

Il nodo del cluster ' %1' è terminato con lo stato isolato.

### <a name="event-1682-cluster_group_moved_no_longer_unmonitored"></a>Evento 1682: CLUSTER_GROUP_MOVED_NO_LONGER_UNMONITORED

È stato eseguito il failover della macchina virtuale ' %2' di cui non è stato eseguito il monitoraggio nel nodo isolato ' %1' in un altro nodo. Il monitoraggio dell'integrità della macchina virtuale è ancora una volta.

### <a name="event-1682-cluster_multiple_groups_no_longer_unmonitored"></a>Evento 1682: CLUSTER_MULTIPLE_GROUPS_NO_LONGER_UNMONITORED

Il nodo ' %1' è stato aggiunto di nuovo al cluster e le macchine virtuali seguenti in esecuzione su tale nodo hanno nuovamente monitorato lo stato di integrità: %2.

### <a name="event-4621-nodecleanup_success"></a>Evento 4621: NODECLEANUP_SUCCESS

Il nodo è stato rimosso dal cluster.

### <a name="event-5121-dcm_volume_no_direct_io_due_to_failure"></a>Evento 5121: DCM_VOLUME_NO_DIRECT_IO_DUE_TO_FAILURE

Il Volume condiviso cluster ' %1' (' %2') non è più accessibile direttamente da questo nodo del cluster. L'accesso i/O verrà reindirizzato al dispositivo di archiviazione attraverso la rete al nodo proprietario del volume. Se si verifica un peggioramento delle prestazioni, risolvere il problema di connettività del nodo al dispositivo di archiviazione e l'I/O riprenderà in uno stato integro una volta ristabilita la connettività al dispositivo di archiviazione.

### <a name="event-5218-csv_old_sw_snapshot_deleted"></a>Evento 5218: CSV_OLD_SW_SNAPSHOT_DELETED

La risorsa disco fisico del cluster ' %1' ha eliminato uno snapshot software. Lo snapshot del software in Volume condiviso cluster ' %2' è stato eliminato perché era più vecchio di ' %3' giorni. L'ID dello snapshot è' %4' ed è stato creato dal nodo ' %5' in ' %6'.
Si prevede che gli snapshot vengano eliminati da un'applicazione di backup dopo il completamento di un processo di backup. Questo snapshot ha superato il tempo previsto per l'esistenza di uno snapshot. Verificare con l'applicazione di backup che i processi di backup vengano completati correttamente.

## <a name="see-also"></a>Vedi anche

-   [Informazioni dettagliate sugli eventi per i componenti del clustering di failover in Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753362(v%3dws.10))
