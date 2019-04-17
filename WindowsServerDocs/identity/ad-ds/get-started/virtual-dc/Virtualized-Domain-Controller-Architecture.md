---
ms.assetid: 341614c6-72c2-444f-8b92-d2663aab7070
title: Architettura di Controller di dominio virtualizzati
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac8b190df065547d82aa431761eb5c00c94a2ad6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-architecture"></a>Architettura di Controller di dominio virtualizzati

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento illustra l'architettura di clonazione del controller di dominio virtualizzati e ripristino sicuro. Visualizza i processi di clonazione e ripristino sicuro con diagrammi di flusso e quindi fornisce una spiegazione dettagliata di ogni passaggio del processo.  
  
-   [Architettura di clonazione del controller di dominio virtualizzati](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch)  
  
-   [Architettura di ripristino sicuro di controller di dominio virtualizzati](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)  
  
## <a name="BKMK_CloneArch"></a>Architettura di clonazione del controller di dominio virtualizzati  
  
### <a name="overview"></a>Panoramica  
Clonazione del controller di dominio virtualizzati basata sulla piattaforma hypervisor per esporre un identificatore chiamato **ID di generazione VM** per rilevare la creazione di una macchina virtuale. Active Directory archivia inizialmente il valore di questo identificatore nel proprio database (NTDS. DIT) durante la promozione del controller di dominio. Quando la macchina virtuale viene avviato, il valore corrente dell'ID di generazione macchina virtuale dalla macchina virtuale viene confrontato con il valore nel database. Se i due valori sono diversi, il controller di dominio Reimposta l'ID di chiamata e rimuove il pool di RID, impedendo in tal modo riutilizzo di numeri USN o la potenziale creazione di entità di sicurezza duplicate. Il controller di dominio quindi cerca un file DCCloneConfig.xml nei percorsi indicati nel passaggio 3 in [dettagliato di clonazione](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneProcessDetails). Se trova un file DCCloneConfig.xml, conclude che è stato distribuito come clone, quindi avvia la clonazione effettuare il provisioning di se stesso come controller di dominio aggiuntivo tramite nuovamente promozione NTDS esistente. Contenuto DIT e SYSVOL copiati dai supporti di origine.  
  
In un ambiente misto in cui alcuni hypervisor supportano l'ID di generazione VM mentre altri no, è possibile che un supporto clone venga accidentalmente distribuito in un hypervisor che non supporta l'ID di generazione VM. La presenza di file DCCloneConfig.xml indica lo scopo amministrativo di clonare un controller di dominio. Pertanto, se durante l'avvio, ma un ID di generazione VM viene individuato un file DCCloneConfig.xml non viene fornito dall'host, il controller di dominio clone viene avviato in Directory Services modalità di ripristino per evitare qualsiasi impatto al resto dell'ambiente. Il supporto clone può essere seguito spostato in un hypervisor che supporta l'ID di generazione VM e quindi la clonazione è possibile ritentare.  
  
Se il supporto clone viene distribuito in un hypervisor che supporta l'ID di generazione VM, ma non viene fornito un file DCCloneConfig.xml, come il controller di dominio rileva una modifica dell'ID di generazione VM tra proprio file DIT e quello della nuova VM, attiverà delle misure di sicurezza per impedire riutilizzo di numeri USN ed evitare SID duplicati. Tuttavia, la clonazione non verrà avviata, in modo che il controller di dominio secondario continuerà a essere eseguito con la stessa identità di controller di dominio di origine. Questo controller di dominio secondario deve essere rimosso dalla rete la prima volta per evitare incoerenze nell'ambiente. Per ulteriori informazioni su come recuperare questo controller di dominio secondario assicurandosi che gli aggiornamenti vengano replicati in uscita, vedere l'articolo della Microsoft KB al contempo l'articolo [2742970](https://support.microsoft.com/kb/2742970).  
  
### <a name="BKMK_CloneProcessDetails"></a>Dettagliato di clonazione  
Il diagramma seguente è illustrata l'architettura per un'operazione di clonazione iniziale e per un'operazione di nuovo tentativo di clonazione. I processi sono illustrati in dettaglio più avanti in questo argomento.  
  
**Operazione di clonazione iniziale**  
  
![Architettura di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_InitialCloningProcess.png)  
  
**Operazione di nuovo tentativo di clonazione**  
  
![Architettura di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_CloningRetryProcess.png)  
  
I passaggi seguenti illustrano il processo in maggiore dettaglio:  
  
1.  Viene avviato un controller di dominio di macchina virtuale esistente in un hypervisor che supporta l'ID di generazione VM.  
  
    1.  Questa macchina virtuale non è impostato alcun valore di ID di generazione VM esistente sull'oggetto computer di dominio Active Directory dopo l'innalzamento di livello.  
  
    2.  Anche se è null, la creazione del computer successivo implica comunque una clonazione, come un nuovo ID di generazione VM non corrispondono.  
  
    3.  L'ID di generazione VM viene impostato dopo il successivo riavvio del controller di dominio e non viene replicato.  
  
2.  La macchina virtuale legge quindi l'ID di generazione VM fornito dal driver VMGenerationCounter. Confronta i due ID di generazione VM.  
  
    1.  Se gli ID corrispondono, non si tratta di una nuova macchina virtuale e la clonazione non proseguirà. Se esiste un file DCCloneConfig.xml, il controller di dominio lo Rinomina con un timestamp per evitare la clonazione. Il server continua normalmente l'avvio. Questa è la modalità di funzionamento di ogni riavvio di qualsiasi controller di dominio virtuale in Windows Server 2012.  
  
    2.  Se i due ID non corrispondono, questa è una nuova macchina virtuale che contiene un NTDS. DIT da un precedente controller di dominio (o è uno snapshot ripristinato). Se esiste un file DCCloneConfig.xml, il controller di dominio prosegue con operazioni di clonazione. In caso contrario, continua con le operazioni di ripristino di snapshot. Vedere [architettura di ripristino sicuro di controller di dominio virtualizzati](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).  
  
    3.  Se l'hypervisor non fornisce un ID di generazione VM per il confronto, ma esiste un file DCCloneConfig.xml, il guest Rinomina il file e quindi viene avviato in modalità ripristino servizi directory per proteggere la rete da un controller di dominio duplicato. Se esiste un file dccloneconfig.xml, il guest viene avviato normalmente (con il rischio di un controller di dominio duplicato sulla rete). Per ulteriori informazioni su come recuperare questo controller di dominio duplicato, Microsoft vedere l'articolo [2742970](https://support.microsoft.com/kb/2742970).  
  
3.  Il servizio NTDS controlla il valore del nome VDCisCloning DWORD del Registro di sistema valore (in HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters.).  
  
    1.  Se non esiste, questo è un primo tentativo di clonazione per questa macchina virtuale. Il guest implementa le misure di sicurezza duplicazione oggetto controller di dominio virtuale di invalidare il pool di RID locale e l'impostazione di un nuovo ID di chiamata di replica per il controller di dominio  
  
    2.  Se è già impostato su 0x1, questa è una clonazione "Riprova" in cui una precedente operazione non riuscita. Le misure di sicurezza duplicazione oggetto controller di dominio virtuale non vengono considerate in quanto dovevano già stata eseguita una volta prima e modificherebbero inutilmente il guest più volte.  
  
4.  Il nome del valore del Registro di sistema IsClone DWORD viene scritto (in HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters.)  
  
5.  Il servizio NTDS cambia il flag di avvio del guest per l'avvio in modalità ripristino servizi directory per tutti i futuri riavvii.  
  
6.  Il servizio NTDS tenta di leggere il file DcCloneConfig.xml in uno dei tre percorsi consentiti (Directory di lavoro DSA, windir%\NTDS o supporto rimovibile di lettura/scrittura, in ordine di lettera di unità, alla radice dell'unità).  
  
    1.  Se il file non esiste in nessun percorso valido, il guest verifica se l'indirizzo IP per la duplicazione. Se l'indirizzo IP non è duplicato, il server viene avviato normalmente. Se è presente un indirizzo IP duplicato, il computer viene avviato in modalità ripristino servizi directory per proteggere la rete da un controller di dominio duplicato.  
  
    2.  Se il file esiste in un percorso valido, il servizio NTDS ne convalida le impostazioni. Se il file è vuoto (o alcune specifiche impostazioni sono vuote) il servizio NTDS Configura valori automatici per tali impostazioni.  
  
    3.  Se il file DcCloneConfig.xml esiste ma contiene voci non valide o è illeggibile, la clonazione non riesce e il guest viene avviato in modalità servizi Directory ripristinare (DSRM).  
  
7.  Il guest Disabilita tutti registrazione automatica di DNS per impedire Hijack accidentali del nome di computer di origine e gli indirizzi IP.  
  
8.  Il guest Arresta il servizio Accesso rete per evitare annunci o risposte alle richieste di rete AD DS dai client.  
  
9. Il servizio NTDS verifica che siano servizi o programmi installati che non fanno parte del file DefaultDCCloneAllowList.xml o CustomDCCloneAllowList.xml  
  
    1.  Se sono disponibili servizi o programmi installati non inclusi nel predefinito elenco consentono o elenco Consenti l'esclusione personalizzato, la clonazione non riesce e il guest viene avviato in modalità ripristino servizi directory per proteggere la rete da un controller di dominio duplicato.  
  
    2.  Se ci sono incompatibilità, la clonazione prosegue.  
  
10. Se verranno utilizzati gli indirizzi IP automatici a causa di impostazioni di rete DCCloneConfig.xml vuote, il guest Abilita DHCP nelle schede di rete per ottenere un IP lease di indirizzo, il routing di rete e informazioni sulla risoluzione dei nome.  
  
11. Il guest individua e contatta il controller di dominio che eseguono l'emulatore PDC ruolo FSMO. Usa DNS e il protocollo DCLocator. Effettua una connessione RPC e chiama il metodo IDL_DRSAddCloneDC per clonare l'oggetto computer controller di dominio.  
  
    1.  Se l'oggetto computer di origine del guest detiene dominio autorizzazione estesa dell'intestazione di "' Consenti un controller di dominio di creare un clone di se stesso", la clonazione prosegue.  
  
    2.  Se l'oggetto computer di origine del guest non detiene che autorizzazione estesa, la clonazione non riesce e il guest viene avviato in modalità ripristino servizi directory per proteggere la rete da un controller di dominio duplicato.  
  
12. Il nome dell'oggetto computer di dominio Active Directory è impostato in base al nome specificato nel file DCCloneConfig.xml, se presente, altrimenti generato automaticamente nell'emulatore PDC. NTDS crea l'oggetto impostazioni NTDS corretto per il sito logico di Active Directory appropriato.  
  
    1.  Se si tratta di un controller di clonazione, il guest Rinomina il computer locale e viene riavviato. Dopo il riavvio, passa attraverso il passaggio 1-10 nuovamente, quindi procede al passaggio 13.  
  
    2.  Se si tratta di una controller di dominio di replica la clonazione, non esiste nessun riavvio in questa fase.  
  
13. Il guest fornisce le impostazioni di promozione al servizio ruolo Server DS, che avvia la promozione.  
  
14. Il servizio ruolo Server DS Arresta tutti i servizi di directory correlati a Active Directory (NTDS, NTFRS/DFSR, KDC, DNS).  
  
15. Il guest forza NT5DS sincronizzazione dell'ora (Windows NTP) con un altro controller di dominio (in una gerarchia di servizio ora di Windows predefinita, questo significa uso dell'emulatore PDC). Il guest contatta l'emulatore PDC. Scaricare tutti i ticket Kerberos esistenti.  
  
16. Il guest configura i servizi DFSR o NTFRS per l'esecuzione automatica. Il guest Elimina tutti DFSR e NTFRS file di database esistenti (impostazione predefinita: c:\windows\ntfrs e c:\system information\dfsr\\ volume*< guid_database >*) per forzare una sincronizzazione non autorevole di SYSVOL quando il servizio viene avviato successivamente. Il guest non elimina il contenuto del file di SYSVOL, di pre-seeding di SYSVOL quando la sincronizzazione viene avviato successivamente.  
  
17. Il guest viene rinominato. Il servizio ruolo Server DS nel guest inizia configurazione di Active Directory (promozione) usando il NTDS esistente. File di database DIT come un'origine, invece che del database modello incluso in c:\windows\system32 come una promozione normalmente.  
  
18. Il guest contatta il detentore del ruolo FSMO Master RID per ottenere una nuova allocazione del pool RID.  
  
19. Il processo di promozione crea un nuovo ID di chiamata e ricrea l'oggetto impostazioni NTDS per il controller di dominio clonato (indipendentemente dalla clonazione, questo fa parte della promozione del dominio quando si utilizza un NTDS esistente. Database DIT).  
  
20. NTDS viene replicato in oggetti mancanti, più recenti, o con una versione successiva da un controller di dominio partner. NTDS. DIT contiene già oggetti dal momento in cui il controller di dominio di origine è passato offline e quelli vengono usati per quanto possibile per ridurre al minimo il traffico di replica in ingresso. Le partizioni del catalogo globale vengono popolate.  
  
21. Il servizio Replica DFS o replica file viene avviata e perché non esiste modo non autorevole Nessun database, SYSVOL Sincronizza in ingresso da un partner di replica. Il processo riusa dati preesistenti nella cartella SYSVOL, per ridurre al minimo il traffico di replica di rete.  
  
22. Il guest riabilita la registrazione del client DNS ora che il computer è denominato in modo univoco e in rete.  
  
23. Il guest esegue i moduli SYSPREP specificati dal DefaultDCCloneAllowList.xml <SysprepInformation> elemento per scrubbing dei riferimenti al nome computer precedente e al SID.  
  
24. La promozione della clonazione è stata completata.  
  
    1.  Il guest rimuove il flag di avvio in modalità ripristino servizi directory in modo che il successivo riavvio venga eseguito normalmente.  
  
    2.  Il guest Rinomina il file DCCloneConfig.xml aggiungendo un indicatore di data e ora aggiunto, in modo che non venga letto di nuovo all'avvio successivo.  
  
    3.  Il guest rimuove il nome del valore del Registro di sistema VdcIsCloning DWORD in HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters..  
  
    4.  Il guest imposta "VdcCloningDone" DWORD nome del valore del Registro di sistema in HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters. 0x1. Windows non utilizzare questo valore, ma fornisce come indicatore per terze parti.  
  
25. Il guest Aggiorna l'attributo msDS-GenerationID nel proprio oggetto controller di dominio clonato in modo che corrisponda il guest corrente ID di generazione VM.  
  
26. Il guest viene riavviato. Ora è un normale pubblicità controller di dominio.  
  
## <a name="BKMK_SafeRestoreArch"></a>Architettura di ripristino sicuro di controller di dominio virtualizzati  
  
### <a name="overview"></a>Panoramica  
Servizi di dominio Active Directory si basa sulla piattaforma hypervisor per esporre un identificatore chiamato **ID di generazione VM** per rilevare il ripristino dello snapshot di una macchina virtuale. Active Directory archivia inizialmente il valore di questo identificatore nel proprio database (NTDS. DIT) durante la promozione del controller di dominio. Quando un amministratore Ripristina la macchina virtuale da uno snapshot precedente, il valore corrente dell'ID di generazione macchina virtuale dalla macchina virtuale viene confrontato con il valore nel database. Se i due valori sono diversi, il controller di dominio Reimposta l'ID di chiamata e rimuove il pool di RID, impedendo in tal modo riutilizzo di numeri USN o la potenziale creazione di entità di sicurezza duplicate. Esistono due scenari in cui può verificarsi un ripristino sicuro:  
  
-   Quando un controller di dominio virtuale viene avviato dopo il ripristino di uno snapshot mentre era arrestato.  
  
-   Quando uno snapshot viene ripristinato in un controller di dominio virtuale in esecuzione  
  
    Se il controller di dominio virtualizzato nello snapshot è in un stato sospeso invece che arrestato, è necessario riavviare il servizio di dominio Active Directory per attivare una nuova richiesta di pool RID. È possibile riavviare il servizio di dominio Active Directory utilizzando lo snap-in servizi o tramite Windows PowerShell (Restart-Service NTDS-force).  
  
Le sezioni seguenti illustrano in dettaglio per ogni scenario il ripristino sicuro.  
  
### <a name="safe-restore-detailed-processing"></a>Processo dettagliato di ripristino sicuro  
Il diagramma seguente mostra ripristino sicuro di come si verifica quando un controller di dominio virtuale viene avviato dopo il ripristino di uno snapshot mentre era arrestato..  
  
![Architettura di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringNormalBoot.png)  
  
1.  Quando la macchina virtuale viene avviata dopo il ripristino di uno snapshot, avrà nuovo ID di generazione VM fornito dall'host dell'hypervisor a causa del ripristino di snapshot.  
  
2.  Il nuovo ID di generazione macchina virtuale dalla macchina virtuale viene confrontato con l'ID di generazione VM nel database. Poiché i due ID non corrispondono, che utilizza misure di sicurezza della virtualizzazione (vedere il passaggio 3 nella sezione precedente). Dopo il ripristino termine dell'applicazione, l'ID di generazione VM impostato sull'oggetto computer di dominio Active Directory viene aggiornato in modo corrisponda al nuovo ID fornito dall'host dell'hypervisor.  
  
3.  Il guest applica le misure di sicurezza della virtualizzazione da:  
  
    1.  Invalida il pool di RID locale.  
  
    2.  L'impostazione di un nuovo ID di chiamata per il database del controller di dominio.  
  
> [!NOTE]  
> Questa parte del ripristino sicuro si sovrappone al processo di clonazione. Anche se il processo riguarda il ripristino sicuro di un controller di dominio virtuale dopo l'avvio in seguito al ripristino di uno snapshot, gli stessi passaggi si verificano durante il processo di clonazione.  
  
Il diagramma seguente mostra come misure di sicurezza della virtualizzazione impediscono la divergenza indotta dal rollback degli USN quando uno snapshot viene ripristinato in un controller di dominio virtuale in esecuzione.  
  
![Architettura di controller di dominio virtualizzati](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringSnapShotRestore.png)  
  
> [!NOTE]  
> Nella figura precedente è stata semplificata per illustrare i concetti.  
  
1.  All'ora T1, l'amministratore dell'hypervisor acquisisce uno snapshot di controller di dominio DC1. In quel momento DC1 ha un valore USN (**highestCommittedUsn** in pratica) del valore di InvocationId (rappresentato come ID nel diagramma precedente) 100, di (in pratica questo sarebbe il GUID). Il valore savedVMGID è l'ID di generazione VM nel file DIT del controller di dominio (archiviato per l'oggetto computer del controller di dominio in un attributo denominato **msDS-GenerationId**). VMGID è il valore corrente dell'ID di generazione VM disponibile dal driver della macchina virtuale. Questo valore viene fornito dall'hypervisor.  
  
2.  Un'ora T2 successiva, vengono aggiunti 100 utenti per questo controller di dominio (prendere in considerazione gli utenti come un esempio di aggiornamenti che potrebbero essere stati eseguiti nel controller di dominio tra ora T1 e T2; questi aggiornamenti potrebbero in realtà essere una combinazione di creazioni utente, gruppi, gli aggiornamenti delle password, aggiornamenti di attributi e così via). In questo esempio ogni aggiornamento Usa un numero USN univoco (anche se in pratica la creazione di un utente potrebbe utilizzare più di uno). Prima di eseguire il commit di questi aggiornamenti, DC1 controlla se il valore dell'ID di generazione VM nel proprio database (savedVMGID) è uguale al valore corrente disponibile nel driver (VMGID). Questi valori sono identici, come verificato nessun rollback ancora, in modo che gli aggiornamenti sono vincolati e il numero USN aumenta a 200, che indica che il successivo aggiornamento può usare l'USN 201. Non è stata modificata in InvocationId, savedVMGID o VMGID. Questi aggiornamenti vengono replicati in DC2 al successivo ciclo di replica. DC2 aggiorna il limite massimo (e **UptoDatenessVector**) rappresentato qui semplicemente come DC1(A) @USN = 200. DC2 è compatibile con tutti gli aggiornamenti di DC1 nel contesto di InvocationId A fino a USN 200.  
  
3.  All'ora T3, lo snapshot acquisito all'ora T1 viene applicato a DC1. DC1 è stato eseguito il rollback, in modo relativo USN viene ripristinato su 100, che indicano che è possibile usare gli USN da 101 per associarli agli aggiornamenti successivi. Tuttavia, a questo punto, il valore di VMGID sarebbe diverso negli hypervisor che supporta l'ID di generazione VM.  
  
4.  Successivamente, quando DC1 esegue un aggiornamento, verifica se il valore di ID di generazione VM presente nel proprio database (savedVMGID) è uguale al valore del driver della macchina virtuale (VMGID). In questo caso, non è lo stesso, quindi DC1 deduce che ciò sia indicativo di un rollback e attiva misure di sicurezza della virtualizzazione; in altri termini, reimposta l'ID di chiamata (ID = B) e rimuove il pool di RID (non illustrato nel diagramma precedente). Quindi Salva il nuovo valore di VMGID nel proprio database ed esegue il commit di tali aggiornamenti (USN 101 - 250) nel contesto del nuovo B. InvocationId Al successivo ciclo di replica, DC2 dispone di alcuna informazione da DC1 nel contesto di InvocationId B, pertanto richiede tutti gli elementi da DC1 associato InvocationID B. Di conseguenza, gli aggiornamenti eseguiti in DC1 dopo l'applicazione dello snapshot convergeranno correttamente. Inoltre, il set di aggiornamenti che sono stati eseguiti in DC1 all'ora T2 (che sono stati persi in DC1 dopo il ripristino dello snapshot) verrebbe replicato di nuovo in DC1 alla successiva replica pianificata perché che era stato replicato in DC2 (come indicato dalla linea tratteggiata verso DC1).  
  
Dopo il guest applica le misure di sicurezza della virtualizzazione, il servizio NTDS replica Active Directory le differenze degli oggetti in ingresso non autorevole da un controller di dominio partner. Il vettore di aggiornamento del servizio directory di destinazione viene aggiornato di conseguenza. Quindi il guest Sincronizza SYSVOL:  
  
-   Se si usa il servizio Replica file, il guest Arresta il servizio NTFRS e imposta il valore del Registro di sistema D2 BURFLAGS. Quindi avviare il servizio NTFRS, che la replica non autorevole in ingresso, riutilizzando attuali dati SYSVOL quando possibile.  
  
-   Se con replica DFS, il guest Arresta il servizio Replica DFS ed elimina i file di database DFSR (percorso predefinito: %systemroot%\system volume information\dfsr\\*<database GUID>*). Quindi avviare il servizio Replica DFS, che la replica non autorevole in ingresso, riutilizzando attuali dati SYSVOL quando possibile.  
  
> [!NOTE]  
> -   Se l'hypervisor non fornisce un ID di generazione VM per il confronto, l'hypervisor non supporta misure di sicurezza della virtualizzazione e il guest opererà come un controller di dominio virtualizzati che esegue Windows Server 2008 R2 o versioni precedenti. Il guest implementa la protezione della quarantena con rollback degli USN se è presente un tentativo di avviare la replica con USN che non sono aumentati dopo l'ultimo USN più alto dal controller di dominio partner. Per ulteriori informazioni sulla protezione della quarantena con rollback degli USN, vedere [USN e Rollback degli USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx)  
  


