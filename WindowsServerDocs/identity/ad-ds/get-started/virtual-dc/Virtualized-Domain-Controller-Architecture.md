---
ms.assetid: 341614c6-72c2-444f-8b92-d2663aab7070
title: Architettura di controller di dominio virtualizzati
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e8673b9e66a0aa3b6bea89b91ae5022efb26c65c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390514"
---
# <a name="virtualized-domain-controller-architecture"></a>Architettura di controller di dominio virtualizzati

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento descrive l'architettura della clonazione e del ripristino sicuro dei controller di dominio virtualizzati. Illustra i processi di clonazione e ripristino sicuro con diagrammi di flusso, quindi fornisce una spiegazione dettagliata di ogni passaggio.  
  
-   [Architettura di clonazione di controller di dominio virtualizzati](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch)  
  
-   [Architettura di ripristino sicuro di controller di dominio virtualizzati](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)  
  
## <a name="BKMK_CloneArch"></a>Architettura di clonazione di controller di dominio virtualizzati  
  
### <a name="overview"></a>Panoramica  
Per la clonazione di domini virtualizzati, la piattaforma hypervisor deve esporre un identificatore chiamato **ID di generazione VM** per rilevare la creazione di una macchina virtuale. Servizi di dominio Active Directory archivia inizialmente il valore di questo identificatore nel proprio database (NTDS.DIT) durante la promozione del controller. All'avvio della macchina virtuale, il relativo valore corrente dell'ID di generazione VM viene confrontato con il valore nel database. Se i due valori sono diversi, il controller di dominio reimposta l'ID di chiamata e rimuove il pool di RID, impedendo in questo modo il riutilizzo di numeri USN o la potenziale creazione di entità di sicurezza duplicate. Il controller di dominio cerca quindi un file DCCloneConfig.xml nei percorsi indicati nel passaggio 3, in [Cloning Detailed Processing](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneProcessDetails). Se trova un file DCCloneConfig.xml, conclude che è stato distribuito come clone, quindi avvia la clonazione per effettuare il provisioning di se stesso come controller di dominio aggiuntivo ripetendo la promozione con i contenuti esistenti di NTDS.DIT e SYSVOL copiati dai supporti di origine.  
  
In un ambiente misto in cui alcuni hypervisor supportano l'ID di generazione VM mentre altri no, è possibile che un supporto clone venga accidentalmente distribuito in un hypervisor che non supporta l'ID di generazione VM. La presenza del file DCCloneConfig.xml indica lo scopo amministrativo della clonazione di un controller di dominio. Pertanto, se durante l'avvio viene trovato un file DCCloneConfig.xml, ma l'host non fornisce un ID di generazione VM, il controller di dominio clone viene avviato in modalità ripristino servizi directory per evitare qualsiasi impatto sul resto dell'ambiente. Il supporto clone può essere in seguito spostato in un hypervisor che supporta l'ID di generazione VM, quindi è possibile provare a ripetere la clonazione.  
  
Se il supporto clone viene distribuito in un hypervisor che supporta l'ID di generazione VM, ma non viene fornito un file DCCloneConfig.xml, quando il controller di dominio rileva una modifica dell'ID di generazione VM tra il proprio file DIT e quello della nuova VM, attiverà delle misure di sicurezza per impedire il riutilizzo di numeri USN ed evitare SID duplicati. Tuttavia, la clonazione non verrà avviata, quindi il controller di dominio secondario continuerà a essere eseguito con la stessa identità di quello di origine. Il controller di dominio secondario dovrà essere rimosso dalla rete il prima possibile, per evitare incoerenze nell'ambiente. Per altre informazioni su come recuperare il controller di dominio secondario assicurandosi al contempo che gli aggiornamenti vengano replicati in uscita, vedere l'articolo [2742970](https://support.microsoft.com/kb/2742970)della Microsoft Knowledge Base.  
  
### <a name="BKMK_CloneProcessDetails"></a>Clonazione di elaborazione dettagliata  
Il diagramma seguente mostra l'architettura per l'operazione iniziale e per la ripetizione del tentativo di clonazione. I processi sono illustrati in maggior dettaglio più avanti in questo argomento.  
  
**Operazione di clonazione iniziale**  
  
![Architettura di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_InitialCloningProcess.png)  
  
**Clonazione dell'operazione di ripetizione dei tentativi**  
  
![Architettura di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_CloningRetryProcess.png)  
  
I passaggi seguenti illustrano il processo in maggior dettaglio:  
  
1.  Il controller di dominio di una macchina virtuale esistente viene avviato in un hypervisor che supporta l'ID di generazione VM.  
  
    1.  La VM non ha un valore esistente dell'ID di generazione VM impostato sull'oggetto computer di Servizi di dominio Active Directory dopo la promozione.  
  
    2.  Anche se è Null, la creazione del computer successivo implica comunque una clonazione, in quanto non ci sarà corrispondenza con un nuovo ID di generazione VM.  
  
    3.  L'ID di generazione VM viene impostato dopo il successivo riavvio del controller di dominio e non viene replicato.  
  
2.  La macchina virtuale legge quindi l'ID di generazione VM fornito dal driver VMGenerationCounter e confronta i due ID.  
  
    1.  Se gli ID corrispondono, non si tratta di una nuova macchina virtuale e la clonazione non proseguirà. Se esiste un file DCCloneConfig.xml, il controller di dominio lo rinomina con un timestamp per evitare la clonazione. L'avvio del server continuerà normalmente. Questo è il funzionamento di ogni riavvio di qualsiasi controller di dominio virtuale in Windows Server 2012.  
  
    2.  Se i due ID non corrispondono, significa che c'è una nuova macchina virtuale che contiene un database NTDS.DIT di un precedente controller di dominio (oppure si tratta di uno snapshot ripristinato). Se esiste un file DCCloneConfig.xml, il controller di dominio prosegue con le operazioni di clonazione. In caso contrario, continua con le operazioni di ripristino dello snapshot. Vedere [Virtualized domain controller safe restore architecture](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).  
  
    3.  Se l'hypervisor non fornisce un ID di generazione VM per il confronto, ma esiste un file DCCloneConfig.xml, il guest rinomina il file e quindi viene avviato in modalità ripristino servizi directory per proteggere la rete da un controller di dominio duplicato. Se non esiste nessun file dccloneconfig.xml, il guest viene avviato normalmente, con la possibilità che nella rete venga aggiunto un controller di dominio duplicato. Per altre informazioni su come recuperare questo controller di dominio duplicato, vedere l'articolo [2742970](https://support.microsoft.com/kb/2742970)della Microsoft Knowledge Base.  
  
3.  Il servizio NTDS controlla il valore del nome VDCisCloning DWORD nel Registro di sistema, in HKEY_Local_Machine\System\CurrentControlSet\Services\Ntds\Parameters.  
  
    1.  Se non esiste, si tratta del primo tentativo di clonazione per questa macchina virtuale. Il guest implementa le misure di sicurezza per la duplicazione di oggetti controller di dominio virtuale, invalidando il pool di RID locale e impostando un nuovo ID di chiamata di replica per il controller di dominio.  
  
    2.  Se è già impostato su 0x1, si tratta di una ripetizione del tentativo di clonazione in seguito a una precedente operazione non riuscita. Le misure di sicurezza per la duplicazione di oggetti controller di dominio virtuale non vengono intraprese, in quanto dovevano essere già state eseguite una volta in precedenza e modificherebbero inutilmente più volte il guest.  
  
4.  Il nome del valore del Registro di sistema IsClone DWORD viene scritto in Hkey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters.  
  
5.  Il servizio NTDS cambia il flag di avvio del guest per attivare la modalità ripristino servizi di directory per tutti i futuri riavvii.  
  
6.  Il servizio NTDS prova a leggere il file DcCloneConfig.xml in uno dei tre percorsi consentiti, ossia directory di lavoro DSA, %windir%\NTDS o supporto rimovibile di lettura/scrittura, nell'ordine di lettera di unità, alla radice dell'unità.  
  
    1.  Se il file non esiste in nessun percorso valido, il guest verifica se l'indirizzo IP è duplicato. Se l'indirizzo IP non è duplicato, il server viene avviato normalmente. Se invece c'è un indirizzo IP duplicato, il computer viene avviato in modalità ripristino servizi directory per proteggere la rete da un controller di dominio duplicato.  
  
    2.  Se il file esiste in un percorso valido, il servizio NTDS ne convalida le impostazioni. Se il file è vuoto (o alcune specifiche impostazioni sono vuote) il servizio NTDS configura valori automatici per queste impostazioni.  
  
    3.  Se il file DcCloneConfig.xml esiste, ma contiene voci non valide o è illeggibile, la clonazione non riesce e il guest viene avviato in modalità ripristino servizi directory.  
  
7.  Il guest disabilita tutta la registrazione automatica di DNS per impedire hijack accidentali del nome computer di origine e degli indirizzi IP.  
  
8.  Il guest arresta il servizio Accesso rete per evitare annunci o risposte alle richieste di Servizi di dominio Active Directory di rete da parte dei client.  
  
9. Il servizio NTDS verifica che non siano installati servizi o programmi che non fanno parte del file DefaultDCCloneAllowList.xml o CustomDCCloneAllowList.xml  
  
    1.  Se sono installati servizi o programmi non inclusi nell'elenco predefinito o personalizzato di elementi esclusi/consentiti, la clonazione non riesce e il guest viene avviato in modalità ripristino servizi directory per proteggere la rete da un controller di dominio duplicato.  
  
    2.  Se non ci sono incompatibilità, la clonazione prosegue.  
  
10. Se verrà usata l'impostazione automatica di indirizzi IP, a causa di impostazioni di rete vuote in DCCloneConfig.xml, il guest abilita DHCP nelle schede di rete per ottenere un lease di indirizzi IP, il routing di rete e informazioni sulla risoluzione dei nomi.  
  
11. Il guest individua e contatta il controller di dominio che esegue il ruolo FSMO dell'emulatore PDC e usa DNS e il protocollo DCLocator. Effettua una connessione RPC e chiama il metodo IDL_DRSAddCloneDC per clonare l'oggetto computer controller di dominio.  
  
    1.  Se l'oggetto computer di origine del guest detiene l'autorizzazione estesa dell'intestazione di dominio "Consenti a un controller di dominio di creare un clone di se stesso", la clonazione prosegue.  
  
    2.  Se l'oggetto computer di origine del guest non detiene questa autorizzazione estesa, la clonazione non riesce e il guest viene avviato in modalità ripristino servizi directory per proteggere la rete da un controller di dominio duplicato.  
  
12. Il nome dell'oggetto computer di Servizi di dominio Active Directory è impostato in modo che corrisponda al nome specificato nel file DCCloneConfig.xml, se presente, o altrimenti che venga generato automaticamente nell'emulatore PDC. NTDS crea l'oggetto impostazione NTDS corretto per il sito logico appropriato di Active Directory.  
  
    1.  Se si tratta di una clonazione PDC, il guest rinomina il computer locale e si riavvia. Dopo il riavvio, passa attraverso il passaggio 1-10 nuovamente, quindi procede al passaggio 13.  
  
    2.  Se si tratta di una clonazione del controller di dominio di replica, non viene effettuato nessun riavvio in questa fase.  
  
13. Il guest fornisce le impostazioni di promozione al servizio Ruolo server DS, che avvia la promozione.  
  
14. Il servizio Ruolo server DS arresta tutti i servizi correlati a Servizi di dominio Active Directory (NTDS, NTFRS/DFSR, KDC, DNS).  
  
15. Il guest forza la sincronizzazione oraria di NT5DS (Windows NTP) con un altro controller di dominio. In una gerarchia predefinita del servizio Ora di Windows, ciò implica l'uso dell'emulatore PDC. Il guest contatta l'emulatore PDC. Tutti gli attuali ticket Kerberos vengono scaricati.  
  
16. Il guest configura i servizi DFSR o NTFRS per l'esecuzione automatica. Il guest Elimina tutti DFSR e NTFRS file di database esistenti (impostazione predefinita: c:\windows\ntfrs e c:\system volume information\dfsr\\ *< database_GUID >* ), in modo da forzare la sincronizzazione non autorevole di SYSVOL quando il servizio viene avviato successivamente. Il guest non elimina il contenuto dei file di SYSVOL, in modo da eseguire il pre-seeding di SYSVOL quando in seguito viene avviata la sincronizzazione.  
  
17. Il guest viene rinominato. Il servizio Ruolo server DS nel guest inizia la configurazione di Servizi di dominio Active Directory (promozione) usando il file di database NTDS.DIT esistente come origine, invece del database modello incluso in c:\windows\system32 come avviene normalmente in una promozione.  
  
18. Il guest contatta il detentore del ruolo FSMO Master RID per ottenere una nuova allocazione del pool di RID.  
  
19. Il processo di promozione crea un nuovo ID di chiamata e ricrea l'oggetto impostazioni NTDS per il controller di dominio clonato. Indipendentemente dalla clonazione, questo passaggio fa parte della promozione del dominio quando si usa un database NTDS.DIT esistente.  
  
20. NTDS viene replicato in oggetti mancanti, più recenti o con un numero di versione più alto da un controller di dominio partner. Il database NTDS.DIT contiene già oggetti dal momento in cui il controller di dominio di origine è passato offline, che vengono usati per quanto possibile al fine di ridurre al minimo in traffico di replica in ingresso. Le partizioni del catalogo globale vengono popolate.  
  
21. Viene avviato il servizio Replica DFS o Replica file e, poiché non c'è nessun database, SYSVOL sincronizza in modo non autorevole il traffico in ingresso proveniente da un partner di replica. Il processo riusa dati preesistenti nella cartella SYSVOL per ridurre al minimo il traffico di replica in rete.  
  
22. Il guest riabilita la registrazione dei client DNS ora che il computer ha un nome e un indirizzo di rete univoci.  
  
23. Il guest esegue i moduli SYSPREP specificati dall'elemento <SysprepInformation> del file DefaultDCCloneAllowList.xml per eseguire lo scrubbing dei riferimenti al nome computer e al SID precedenti.  
  
24. La promozione della clonazione è a questo punto completata.  
  
    1.  Il guest rimuove il flag di avvio in modalità ripristino servizi directory in modo che il successivo riavvio venga eseguito normalmente.  
  
    2.  Il guest rinomina il file DCCloneConfig.xml aggiungendo un timestamp alla fine in modo che non venga letto di nuovo al successivo avvio.  
  
    3.  Il guest rimuove il valore del Registro di sistema VDCisCloning DWORD in HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters.  
  
    4.  Il guest imposta su 0x1 il nome del valore del Registro di sistema "VdcCloningDone" DWORD in HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters. Windows non usa questo valore, ma lo fornisce come indicatore per terze parti.  
  
25. Il guest aggiorna l'attributo msDS-GenerationID nel proprio oggetto controller di dominio clonato in modo che corrisponda all'ID di generazione VM corrente.  
  
26. Il guest viene riavviato. A questo punto si tratta di un normale controller di dominio di annunci.  
  
## <a name="BKMK_SafeRestoreArch"></a>Architettura di ripristino sicuro di controller di dominio virtualizzati  
  
### <a name="overview"></a>Panoramica  
Servizi di dominio Active Directory si basa sulla piattaforma hypervisor per esporre un identificatore chiamato **ID di generazione VM** e rilevare il ripristino dello snapshot di una macchina virtuale. Servizi di dominio Active Directory archivia inizialmente il valore di questo identificatore nel proprio database (NTDS.DIT) durante la promozione del controller. Quando un amministratore ripristina la macchina virtuale da uno snapshot precedente, il valore corrente dell'ID di generazione VM ottenuto dalla macchina virtuale viene confrontato con un valore nel database. Se i due valori sono diversi, il controller di dominio reimposta l'ID di chiamata e rimuove il pool di RID, impedendo in questo modo il riutilizzo di numeri USN o la potenziale creazione di entità di sicurezza duplicate. Esistono due scenari in cui può verificarsi un ripristino sicuro:  
  
-   Quando un controller di dominio virtuale viene avviato dopo il ripristino di uno snapshot mentre era arrestato.  
  
-   Quando uno snapshot viene ripristinato in un controller di dominio virtuale in esecuzione.  
  
    Se il controller di dominio virtualizzato nello snapshot è in uno stato sospeso invece che arrestato, è necessario riavviare il servizio Servizi di dominio Active Directory per avviare una nuova richiesta al pool di RID. È possibile riavviare il servizio Servizi di dominio Active Directory usando lo snap-in Servizi oppure Windows PowerShell (Restart-Service NTDS -force).  
  
Le sezioni seguenti illustrano in dettaglio il ripristino sicuro per ogni scenario.  
  
### <a name="safe-restore-detailed-processing"></a>Processo dettagliato di ripristino sicuro  
Il diagramma di flusso seguente illustra come si verifica un ripristino sicuro quando un controller di dominio virtuale viene avviato dopo il ripristino di uno snapshot mentre era arrestato.  
  
![Architettura di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringNormalBoot.png)  
  
1.  Quando la macchina virtuale viene avviata dopo il ripristino di uno snapshot, avrà un nuovo ID di generazione VM fornito dall'host dell'hypervisor a causa del ripristino dello snapshot.  
  
2.  Il nuovo ID di generazione VM della macchina virtuale viene confrontato con quello presente nel database. Poiché i due ID non corrispondono, vengono implementate misure di sicurezza della virtualizzazione (vedere il passaggio 3 della sezione precedente). Al termine dell'applicazione del ripristino, l'ID di generazione VM impostato nel relativo oggetto computer di Servizi di dominio Active Directory viene aggiornato in modo che corrisponda al nuovo ID fornito dall'host dell'hypervisor.  
  
3.  Il guest applica le misure di sicurezza della virtualizzazione in due modi:  
  
    1.  Invalida il pool di RID locale.  
  
    2.  Imposta un nuovo ID di chiamata per il database del controller di dominio.  
  
> [!NOTE]  
> Questa parte del ripristino sicuro si sovrappone al processo di clonazione. Anche se il processo riguarda il ripristino sicuro di un controller di dominio virtuale dopo l'avvio in seguito al ripristino di uno snapshot, gli stessi passaggi si verificano durante il processo di clonazione.  
  
Il diagramma seguente mostra in che modo le misure di sicurezza della virtualizzazione impediscono la divergenza indotta dal rollback degli USN quando uno snapshot viene ripristinato in un controller di dominio in esecuzione.  
  
![Architettura di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringSnapShotRestore.png)  
  
> [!NOTE]  
> La figura precedente è stata semplificata per illustrare i concetti.  
  
1.  All'ora T1, l'amministratore dell'hypervisor acquisisce uno snapshot del controller di dominio DC1. In quel momento DC1 ha un valore di USN (**highestCommittedUsn**) pari a 100 e un valore di InvocationId (rappresentato come ID nel diagramma precedente) A (in pratica questo sarebbe il GUID). Il valore savedVMGID corrisponde all'ID di generazione VM nel file DIT del controller di dominio (archiviato per l'oggetto computer del controller di dominio in un attributo denominato **msDS-GenerationId**). VMGID è il valore corrente dell'ID di generazione VM disponibile nel driver della macchina virtuale. Questo valore viene fornito dall'hypervisor.  
  
2.  A un'ora T2 successiva, in questo controller di dominio vengono aggiunti 100 utenti. Gli utenti possono essere considerati come un esempio degli aggiornamenti che potrebbero essere stati eseguiti in questo controller di dominio tra l'ora T1 e l'ora T2. Questi aggiornamenti potrebbero in realtà essere una combinazione di creazione di utenti o gruppi, aggiornamenti di password e attributi e così via. In questo esempio ogni aggiornamento usa un numero USN univoco, anche se in pratica la creazione di un utente potrebbe usarne più di uno. Prima di eseguire il commit di questi aggiornamenti, il controller di dominio DC1 controlla se il valore dell'ID di generazione VM nel relativo database (savedVMGID) è uguale al valore corrente disponibile nel driver (VMGID). Questi valori sono identici, in quanto non si è ancora verificato nessun rollback, quindi viene eseguito il commit degli aggiornamenti e il numero USN aumenta a 200, a indicare che il successivo aggiornamento può usare l'USN 201. I valori di InvocationId, savedVMGID o VMGID rimangono inalterati. Questi aggiornamenti vengono replicati nel controller di dominio DC2 al successivo ciclo di replica. DC2 aggiorna il limite massimo (e **UptoDatenessVector**), rappresentato qui semplicemente come DC1 (a) @USN = 200. Ossia, DC2 riconosce tutti gli aggiornamenti di DC1 nel contesto di InvocationId A fino a USN 200.  
  
3.  All'ora T3, lo snapshot acquisito all'ora T1 viene applicato al controller di dominio DC1. Poiché è stato eseguito il rollback di DC1, il relativo USN viene ripristinato su 100, a indicare che è possibile usare gli USN a partire da 101 per associarli agli aggiornamenti successivi. Tuttavia, a questo punto il valore di VMGID sarebbe diverso negli hypervisor che supportano l'ID di generazione VM.  
  
4.  In seguito, quando il controller di dominio DC1 esegue un aggiornamento, verifica se il valore di ID di generazione VM presente nel proprio database (savedVMGID) è uguale al valore del driver della macchina virtuale (VMGID). In questo caso non è uguale, quindi DC1 deduce che ciò sia indicativo di un rollback e attiva le misure di sicurezza della virtualizzazione. In altri termini, reimposta il proprio valore di InvocationId (ID = B) e rimuove il pool di RID (non illustrato nel diagramma precedente). Quindi Salva il nuovo valore di VMGID nel proprio database e viene eseguito il commit di tali aggiornamenti (USN 101 - 250) nel contesto del nuovo B. InvocationId Al successivo ciclo di replica, DC2 dispone di alcuna informazione da DC1 nel contesto di InvocationId B, pertanto richiede tutti gli elementi da DC1 associato InvocationID B. Di conseguenza, gli aggiornamenti eseguiti in DC1 dopo l'applicazione dello snapshot convergeranno. Inoltre, il set di aggiornamenti eseguiti in DC1 all'ora T2 (che sono andati persi in DC1 dopo il ripristino dello snapshot) verrebbe replicato di nuovo in DC1 alla successiva replica pianificata, perché era stato replicato in DC2, come indicato dalla linea tratteggiata verso DC1.  
  
Dopo che il guest applica le misure di sicurezza della virtualizzazione, il servizio NTDS replica in modo non autorevole le differenze degli oggetti Active Directory in ingresso provenienti da un controller di dominio partner. Il vettore di aggiornamento del servizio directory di destinazione viene aggiornato di conseguenza. Quindi il guest sincronizza SYSVOL:  
  
-   Se si usa il servizio Replica file, il guest arresta il servizio NTFRS e imposta il valore del Registro di sistema D2 BURFLAGS. Avvia quindi il servizio NTFRS, che esegue la replica non autorevole in ingresso, riutilizzando gli attuali dati SYSVOL non modificati, quando possibile.  
  
-   Se con replica DFS, il guest Arresta il servizio Replica DFS ed elimina i file di database DFSR (percorso predefinito: %systemroot%\system. volume information\dfsr\\ *<database GUID>* ). Avvia quindi il servizio Replica DFS, che esegue la replica non autorevole in ingresso, riutilizzando gli attuali dati SYSVOL non modificati, quando possibile.  
  
> [!NOTE]  
> -   Se l'hypervisor non fornisce un ID di generazione VM per il confronto, significa che non supporta le misure di sicurezza della virtualizzazione e il guest opererà come un controller di dominio virtualizzato che esegue Windows Server 2008 R2 o versione precedente. Il guest implementa la protezione della quarantena con rollback degli USN se si verifica un tentativo di avviare la replica con USN che non sono aumentati dopo l'ultimo USN più alto riscontrato dal controller di dominio partner. Per altre informazioni sulla protezione della quarantena con rollback degli USN, vedere l'articolo su [USN e rollback degli USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx)  
  


