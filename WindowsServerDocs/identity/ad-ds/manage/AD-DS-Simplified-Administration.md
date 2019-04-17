---
ms.assetid: f74eec9a-2485-4ee0-a0d8-cce01250a294
title: Amministrazione semplificata di servizi di dominio Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6232e281c47f3b5b4627bc9d8ccf53269aafc390
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-simplified-administration"></a>Amministrazione semplificata di servizi di dominio Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento illustra le nuove funzionalità e vantaggi della distribuzione del controller di dominio Windows Server 2012 e amministrazione e le differenze tra precedente distribuzione del sistema operativo controller di dominio e la nuova implementazione di Windows Server 2012.  
  
Windows Server 2012 introduce la nuova generazione di Active Directory dominio amministrazione semplificata di servizi ed è l'innovazione più radicale realizzata dopo Windows 2000 Server. Amministrazione semplificata di Active Directory sfrutta appresi dodici anni di Active Directory e ne un'esperienza amministrativa più intuitiva più supportata, flessibile, ad architetti e amministratori. In questo modo la creazione di nuove versioni di tecnologie esistenti oltre all'estensione delle funzionalità dei componenti rilasciati in Windows Server 2008 R2.  
  
Amministrazione semplificata di Active Directory è una nuova forma della distribuzione dei domini.  
  
-   Distribuzione di ruolo di Active Directory fa ora parte della nuova architettura di Server Manager e consente l'installazione remota  
  
-   Il motore di distribuzione e configurazione di dominio Active Directory è ora Windows PowerShell, anche quando si usa la nuova configurazione guidata di Active Directory  
  
-   Estensione dello schema, preparazione della foresta e la preparazione del dominio automaticamente fanno parte di promozione del controller di dominio e non richiedono più attività separate su speciali server come il Master Schema  
  
-   L'innalzamento di livello ora include il controllo dei prerequisiti che convalida la disponibilità di foreste e domini per il nuovo controller di dominio, riducendo la possibilità di innalzamenti di livello riusciti  
  
-   Modulo di Active Directory per Windows PowerShell ora include i cmdlet per la gestione della topologia di replica, controllo dinamico degli accessi e altre operazioni  
  
-   La funzionalità livello non implementa nuove caratteristiche e il livello di funzionalità del dominio è richiesto solo per un subset di nuove caratteristiche Kerberos, sollevando gli amministratori di frequente della foresta Windows Server 2012 necessità di un ambiente di controller di dominio omogeneo  
  
-   Completo aggiunto il supporto per controller di dominio virtualizzati, per includere protezione automatica di distribuzione e il rollback  
  
Per ulteriori informazioni sui controller di dominio virtualizzati, vedere [Introduzione a servizi di dominio Active Directory e 40 #; Servizi di dominio Active Directory & #41; Virtualizzazione & #40; Livello 100 & #41;](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).  
  
Inoltre, esistono molte amministrativi e i miglioramenti di manutenzione:  
  
-   Il centro di amministrazione di Active Directory include una grafica Cestino per Active Directory, la gestione di criteri Password granulari e Visualizzatore della cronologia di Windows PowerShell  
  
-   Il nuovo Server Manager ha interfacce specifiche di AD DS il monitoraggio delle prestazioni, analisi delle procedure consigliate, i servizi critici e i registri eventi  
  
-   Account del servizio gestito del gruppo supportano più computer con le stesse entità di sicurezza  
  
-   Miglioramenti nel rilascio di identificatori relativi (RID) e il monitoraggio per una migliore gestibilità nei domini di Active Directory maturi  
  
Servizi di dominio Active Directory, infine, trae vantaggio da altre nuove funzionalità incluse in Windows Server 2012, ad esempio:  
  
-   Gruppo NIC e Bridging dei Data Center  
  
-   Sicurezza DNS e disponibilità zona integrata in Active Directory più veloce dopo l'avvio  
  
-   Miglioramenti dell'affidabilità e la scalabilità di Hyper-V  
  
-   Sblocco rete con BitLocker  
  
-   Altri moduli di amministrazione di Windows PowerShell componente  
  
## <a name="technical-overview"></a>Panoramica tecnica  
  
### <a name="adprep-integration"></a>Integrazione di ADPREP  
Active Directory dello schema estensione e dominio preparazione della foresta ora integrare il processo di configurazione di controller di dominio. Se si innalza di livello un nuovo controller di dominio in una foresta esistente, il processo individua lo stato dell'aggiornamento e le fasi di preparazione schema estensione e dominio vengono eseguite automaticamente. L'utente che installa il primo controller di dominio di Windows Server 2012 deve comunque essere un Enterprise Admins e Schema Admins o fornire valide credenziali alternative.  
  
Adprep.exe rimane sul DVD per la foresta separata e la preparazione del dominio. La versione dello strumento incluso in Windows Server 2012 è compatibile con le versioni precedenti di Windows Server 2008 x64 e Windows Server 2008 R2. Adprep.exe supporta anche forestprep e domainprep, proprio come gli strumenti di configurazione di controller di dominio basati su ADDSDeployment remoti.  
  
Per informazioni su Adprep e preparazione della foresta precedente sistema operativo, vedere [esecuzione di Adprep (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd464018(WS.10).aspx).  
  
### <a name="server-manager-ad-ds-integration"></a>Integrazione di Server Manager AD DS  
![Amministrazione semplificata](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_Dashboard.png)  
  
Server Manager funge da hub per le attività di gestione server. L'aspetto del dashboard stile vengono periodicamente aggiornate le visualizzazioni dei ruoli installati e gruppi di server remoti. Server Manager consente la gestione centralizzata dei server locali e remoti, senza la necessità di accedere alla console.  
  
Servizi di dominio Active Directory è uno di questi ruoli hub; eseguendo Server Manager in un controller di dominio o strumenti di amministrazione Server remota in un Windows 8, vedrai principali problemi recenti nei controller di dominio della foresta.  
  
Queste visualizzazioni includono:  
  
-   Disponibilità server  
  
-   Avvisi di Performance monitor per un utilizzo elevato della CPU e memoria  
  
-   Lo stato dei servizi di Windows specifici di dominio Active Directory  
  
-   Correlate a servizi di Directory avviso ed errore voci recenti nel registro eventi  
  
-   Analisi delle procedure consigliate di un controller di dominio da un set di regole consigliate di Microsoft  
  
### <a name="active-directory-administrative-center-recycle-bin"></a>Cestino del centro di amministrazione di Active Directory  
![Amministrazione semplificata](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_ADAC.png)  
  
Windows Server 2008 R2 è stato introdotto il Cestino per Active Directory, che recupera gli oggetti di Active Directory eliminati senza il ripristino dal backup, il riavvio del servizio di dominio Active Directory o il riavvio di controller di dominio.  
  
Windows Server 2012 migliora le funzionalità di ripristino basate su Windows PowerShell esistenti con una nuova interfaccia grafica nel centro di amministrazione di Active Directory. Ciò consente agli amministratori di abilitare il Cestino e di individuare o ripristinare gli oggetti eliminati nei contesti del dominio della foresta, senza eseguire direttamente i cmdlet di Windows PowerShell. Il centro di amministrazione di Active Directory e il Cestino per Active Directory tuttavia usano Windows PowerShell dietro le quinte, in modo che le procedure e script precedenti sono ancora utili.  
  
Per informazioni su Active Directory [Cestino, vedere Cestino per Active Directory Step-by-Step Guida (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd392261(WS.10).aspx).  
  
### <a name="active-directory-administrative-center-fine-grained-password-policy"></a>Criteri granulari per le Password centro di amministrazione di Active Directory  
![Amministrazione semplificata](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_FGPP.png)  
  
Windows Server 2008 sono stati introdotti i criteri granulari per le Password, che consentono agli amministratori di configurare più criteri di blocco degli account e password per ogni dominio. Questo offre una soluzione flessibile per applicare le regole di password più o meno restrittive, basate su utenti e gruppi. Non aveva disponibile un'interfaccia e gli amministratori devono configurarli con Ldp.exe o Adsiedit.msc. Windows Server 2008 R2 introdotto il modulo Active Directory per Windows PowerShell, che offre agli amministratori un'interfaccia della riga di comando per Granulari.  
  
Windows Server 2012 offre un'interfaccia grafica per i criteri Password granulari. Il centro di amministrazione di Active Directory è il cuore di questo nuovo dialogo, che offre una gestione semplificata Granulari a tutti gli amministratori.  
  
Per informazioni sulla gestione delle Password, vedere [AD DS granulari per le Password e criterio di blocco Account Step-by-Step Guida (Windows Server 2008 R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx).  
  
### <a name="active-directory-administrative-center-windows-powershell-history-viewer"></a>Visualizzatore della cronologia di PowerShell Windows centro di amministrazione di Active Directory  
![Amministrazione semplificata](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_HistoryViewer.png)  
  
Windows Server 2008 R2 è stato introdotto il centro di amministrazione Active Directory, che ha sostituito il precedente Active Directory Users snap-in e computer creato in Windows 2000. Il centro di amministrazione di Active Directory crea un'interfaccia amministrativa grafica per l'allora nuovo modulo di Active Directory per Windows PowerShell.  
  
Mentre il modulo Active Directory contiene oltre cento cmdlet, la curva di apprendimento per un amministratore può essere ripida. Dato che Windows PowerShell si integra profondamente con la strategia di amministrazione di Windows, il centro di amministrazione di Active Directory include ora un visualizzatore che consente di visualizzare l'esecuzione dei cmdlet nell'interfaccia grafica. È possibile cercare, copiare, cancellare la cronologia e aggiungere note con una semplice interfaccia. Lo scopo è per un amministratore di utilizzare l'interfaccia grafica per creare e modificare gli oggetti e quindi rivederli nel Visualizzatore della cronologia per altre informazioni sull'esecuzione di script Windows PowerShell e modificare gli esempi.  
  
### <a name="ad-replication-windows-powershell"></a>Windows PowerShell con replica Active Directory  
![Amministrazione semplificata](media/AD-DS-Simplified-Administration/ADDS_PSNewADReplSite.png)  
  
Windows Server 2012 aggiunge altri cmdlet di replica di Active Directory per il modulo di Windows PowerShell per Active Directory. Questi consentono la configurazione del nuovi o esistenti siti, subnet, connessioni, collegamenti di sito e Bridge. Restituiscono anche Active Directory i metadati di replica, lo stato della replica, Accodamento messaggi e informazioni sul vettore di versione di aggiornamento. L'introduzione dei cmdlet di replica, combinati con la distribuzione e altri cmdlet di dominio Active Directory esistente - consente di amministrare una foresta solo con Windows PowerShell. Consente di creare nuove opportunità per gli amministratori che desiderano effettuare il provisioning e gestire Windows Server 2012 senza interfaccia grafica, che quindi riduce la superficie di attacco del sistema operativo e i requisiti di manutenzione. Ciò è particolarmente importante quando si distribuiscono server in reti con sicurezza elevata, ad esempio Secret Internet Protocol Router SIPR () e le reti perimetrali aziendali.  
  
Per ulteriori informazioni sulla replica e topologia del sito di Active Directory, vedere il [documentazione tecnica su Windows Server](https://technet.microsoft.com/library/cc739127(WS.10).aspx).  
  
### <a name="rid-management-and-issuance-improvements"></a>Miglioramenti di rilascio e Gestione RID  
Windows 2000 Active Directory è stato introdotto il Master RID, che rilascia pool di identificatori relativi ai controller di dominio, per creare ID di sicurezza (SID) di terze parti trusted di sicurezza come utenti, gruppi e computer.  Per impostazione predefinita, questo spazio RID globale è limitato a 2<sup>30</sup> (o 1.073.741.823) SID totali creati in un dominio. SID non possono restituire al pool o nuovamente. Nel corso del tempo, un dominio di grandi dimensioni può iniziare a rallentare per mancanza di RID o che degli incidenti portino a un consumo di RID non necessario e un eventuale esaurimento.  
  
Indirizzi di Windows Server 2012 con lo sviluppo di un numero di problemi di rilascio e gestione di RID individuati dai clienti e supporto tecnico clienti Microsoft come servizi di dominio Active Directory dopo la creazione dei domini di Active Directory prima nel 1999. Queste includono:  
  
-   Nel registro eventi vengono scritti periodicamente avvisi sull'utilizzo di RID  
  
-   Vengono registrati eventi quando un amministratore invalida un pool di RID  
  
-   I criteri RID che dimensione del blocco di RID viene ora applicato un tetto massimo  
  
-   Limiti massimi di RID artificiali vengono ora applicati e registrati quando lo spazio RID globale è scarso, consentendo a un amministratore di intervenire prima che si esaurisce lo spazio globale  
  
-   Lo spazio RID globale ora può essere incrementato di un bit, raddoppiando la dimensione a 2<sup>31</sup> (2.147.483.648 SID)  
  
Per ulteriori informazioni sui RID e sul Master RID, esaminare [come funzionano gli ID di sicurezza](https://technet.microsoft.com/library/cc778824(WS.10).aspx).  
  
## <a name="new-ad-ds-deployment-architecture"></a>Architettura di distribuzione nuova istanza di AD DS  
  
### <a name="ad-ds-role-deployment-and-management-architecture"></a>Distribuzione di AD DS ruolo e architettura di gestione  
Server Manager e Windows PowerShell ADDSDeployment si basano sui seguenti assembly principali per la funzionalità quando si distribuisce o la gestione del ruolo di dominio Active Directory:  
  
-   Microsoft.ADroles.Aspects.dll  
  
-   Microsoft.ADroles.Instrumentation.dll  
  
-   Microsoft.ADRoles.ServerManager.Common.dll  
  
-   Microsoft.ADRoles.UI. Alla libreria Common.dll  
  
-   Microsoft.DirectoryServices.Deployment.Types.dll  
  
-   Microsoft.DirectoryServices.ServerManager.dll  
  
-   Addsdeployment.psm1  
  
-   Addsdeployment.psd1  
  
Entrambi si basano su Windows PowerShell e il relativo remoto invoke-command per la configurazione e installazione del ruolo remoto.  
  
![Amministrazione semplificata](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_DepDLLs.png)  
  
Windows Server 2012 inoltre effettua il refactoring un numero di operazioni di innalzamento di livello precedente fuori LSASS.EXE, come parte di:  
  
-   Servizio ruolo Server DS (DsRoleSvc)  
  
-   DSRoleSvc.dll (caricata dal servizio DsRoleSvc)  
  
Questo servizio deve essere presente e in esecuzione per alzare di livello, abbassare di livello o clonare i controller di dominio virtuale. Installazione del ruolo di Active Directory aggiunge questo servizio e imposta un tipo di avvio manuale, per impostazione predefinita. Non disabilitare questo servizio.  
  
### <a name="adprep-and-prerequisite-checking-architecture"></a>ADPrep e architettura di controllo dei prerequisiti  
Adprep non deve essere eseguito nel master schema. Può essere eseguita in modalità remota da un computer che esegue Windows Server 2008 x64 o versione successiva.  
  
> [!NOTE]  
> Adprep Usa LDAP per importare i file Schxx.ldf e non si riconnette automaticamente se la connessione al master schema viene persa durante l'importazione. Come parte del processo di importazione, il master schema viene impostato in una modalità specifica e la riconnessione automatica viene disabilitata perché, se LDAP si riconnette dopo la connessione viene persa, la connessione ristabilita non sarà nella modalità specifica. In tal caso, lo schema non verrà aggiornato correttamente.  
  
Controllo dei prerequisiti verifica che determinate condizioni siano vere. Queste condizioni sono obbligatorie per la corretta installazione di servizi di dominio Active Directory. Se alcune condizioni obbligatorie non sono vere, possono essere risolti prima di continuare l'installazione. Rileva che un dominio o foresta non è ancora pronto, in modo che il codice di distribuzione di Adprep viene eseguito automaticamente.  
  
#### <a name="adprep-executables-dlls-ldfs-files"></a>ADPrep Executables, DLL, ldf, file  
  
-   ADprep.dll  
  
-   Ldifde.dll  
  
-   Csvde.dll  
  
-   Sch14.ldf - Sch56.ldf  
  
-   Schupgrade.cat  
  
-   *Dcpromo.csv  
  
Il codice di preparazione di Active Directory in precedenza incluso in ADprep.exe viene effettuato il refactoring in adprep.dll. In questo modo ADPrep.exe e il modulo ADDSDeployment di Windows PowerShell utilizzare la libreria per le stesse attività e avere le stesse funzionalità. Adprep.exe è incluso il supporto di installazione ma processi automatizzati non lo chiamano direttamente - solo un amministratore lo esegue manualmente. È possibile eseguire solo in Windows Server 2008 x64 e sistemi operativi successivi. Ldifde.exe e csvde.exe anche avere effettuato il refactoring versioni come DLL che vengono caricate dal processo di preparazione. Estensione dello schema Usa ancora il firma file LDF con verifica, come nelle versioni precedenti del sistema operativo.  
  
![Amministrazione semplificata](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_AdprepDLLs.png)  
  
> [!IMPORTANT]  
> Non esiste alcun strumento Adprep32.exe a 32 bit per Windows Server 2012. È necessario disporre di almeno un Windows Server 2008 x64, Windows Server 2008 R2 o computer Windows Server 2012, in esecuzione come controller di dominio, server membro, o in un gruppo di lavoro per preparare la foresta e dominio. Adprep.exe non viene eseguito in Windows Server 2003 x64.  
  
#### <a name="BKMK_PrereuisiteChecking"></a>Controllo dei prerequisiti  
Il prerequisito controllo sistema integrato nel codice gestito di Windows PowerShell ADDSDeployment funziona in modalità diverse a seconda dell'operazione. Nelle tabelle seguenti viene descritto ogni test, quando viene usato e informazioni sulla modalità e la convalida. Queste tabelle possono essere utili se sono presenti problemi in cui la convalida non riesce e l'errore non è sufficiente per risolvere il problema.  
  
Questi test vengono registrati **DirectoryServices-Deployment** registro eventi operativi canale sotto la categoria attività **Core**, sempre come ID evento **103**.  
  
##### <a name="prerequisite-windows-powershell"></a>Prerequisiti di Windows PowerShell  
Sono disponibili cmdlet ADDSDeployment di Windows PowerShell per tutti i cmdlet di distribuzione di controller di dominio. Hanno circa gli stessi argomenti dei cmdlet associati.  
  
-   Test-ADDSDomainControllerInstallation  
  
-   Test-ADDSDomainControllerUninstallation  
  
-   Test-ADDSDomainInstallation  
  
-   Test-ADDSForestInstallation  
  
-   Test-ADDSReadOnlyDomainControllerAccountCreation  
  
Non è necessario eseguire questi cmdlet, in genere; vengono già eseguiti automaticamente con i cmdlet di distribuzione per impostazione predefinita.  
  
##### <a name="BKMK_ADDSInstallPrerequisiteTests"></a>Test dei prerequisiti  
  
||||  
|-|-|-|  
|Nome test|Protocolli<br /><br />usato|Spiegazione e note|  
|VerifyAdminTrusted<br /><br />ForDelegationProvider|LDAP|Convalida che si dispone di "Abilita account computer ed utente a tipo trusted per delega" privilegio (SeEnableDelegationPrivilege) sul controller di dominio partner esistente. Necessario l'accesso all'attributo costruito tokenGroups.<br /><br />Non usato quando viene contattato il controller di dominio di Windows Server 2003. È necessario confermare manualmente questo privilegio prima dell'innalzamento di livello|  
|VerifyADPrep<br /><br />Prerequisiti (foresta)|LDAP|Individua e contatta il Master Schema con l'attributo namingContexts di rootDSE e l'attributo fsmoRoleOwner del contesto dei nomi dello Schema. Determina le operazioni preliminari (forestprep, domainprep o rodcprep) necessarie per l'installazione di servizi di dominio Active Directory. Convalida che lo schema objectVersion è previsto e se richiede un'ulteriore estensione.|  
|VerifyADPrep<br /><br />Prerequisiti (dominio e controller di dominio)|LDAP|Individua e contatta il Master infrastrutture con l'attributo namingContexts di rootDSE e l'attributo fsmoRoleOwner del contenitore dell'infrastruttura. Nel caso di un'installazione di controller di dominio, questo test individua master denominazione domini e verifica che sia online.|  
|CheckGroup<br /><br />Appartenenza al gruppo|LDAP,<br /><br />RPC su SMB (LSARPC)|Convalidare l'utente è membro del gruppo Domain Admins o Enterprise Admins, a seconda dell'operazione (DA per l'aggiunta o l'abbassamento di livello un controller di dominio, EA per l'aggiunta o rimozione di un dominio)|  
|CheckForestPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC su SMB (LSARPC)|Convalidare l'utente è membro del gruppo Schema Admins ed Enterprise Admins e disponga di controllo di gestione e del privilegio di registri eventi di sicurezza (SesScurityPrivilege) sui controller di dominio esistente|  
|CheckDomainPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC su SMB (LSARPC)|Convalida che l'utente è membro del gruppo Domain Admins e ha il controllo di gestione e del privilegio di registri eventi di sicurezza (SesScurityPrivilege) sui controller di dominio esistente|  
|CheckRODCPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC su SMB (LSARPC)|Convalida che l'utente è membro del gruppo Enterprise Admins e ha il controllo di gestione e del privilegio di registri eventi di sicurezza (SesScurityPrivilege) sui controller di dominio esistente|  
|VerifyInitSync<br /><br />AfterReboot|LDAP|Verificare che il Master Schema è stato replicato almeno una volta dopo il riavvio impostando un valore fittizio su becomeschemamaster di rootDSE attributo RootDSE.|  
|VerifySFUHotFix<br /><br />Applicato|LDAP|Convalida che la foresta esistente non contiene estensione del problema noto SFU2 per l'attributo UID con OID 1.2.840.113556.1.4.7000.187.102 dello schema<br /><br />([https://support.microsoft.com/kb/821732](https://support.microsoft.com/kb/821732))|  
|VerifyExchange<br /><br />SchemaFixed|LDAP, WMI, DCOM, RPC|Convalida che la foresta esistente dello schema non contengono ancora problema Exchange 2000 estensioni ms-Exch-Assistant-Name, ms-Exch-LabeledURI e ms-Exch-House-Identifier ([https://support.microsoft.com/kb/314649](https://support.microsoft.com/kb/314649))|  
|VerifyWin2KSchema<br /><br />Coerenza|LDAP|Convalida che la foresta esistente schema ha coerenti (non erroneamente modificati da terze parti) attributi e classi principali.|  
|DCPromo|DRSR su RPC,<br /><br />LDAP,<br /><br />DNS<br /><br />RPC su SMB (SAMR)|Convalidare la sintassi della riga di comando passata alla promozione innalzamento di livello di codice e test. Convalida che la foresta o dominio non esiste già se la creazione di nuovi.|  
|VerifyOutbound<br /><br />ReplicationEnabled|LDAP, DRSR su SMB, RPC su SMB (LSARPC)|Convalidare il controller di dominio esistente specificato come partner di replica è abilitata controllando l'attributo delle opzioni dell'oggetto impostazioni NTDS per NTDSDSA_OPT_DISABLE_OUTBOUND_REPL (0x00000004) la replica in uscita|  
|VerifyMachineAdmin<br /><br />Password|DRSR su RPC,<br /><br />LDAP,<br /><br />DNS<br /><br />RPC su SMB (SAMR)|Convalidare la password della modalità provvisoria impostata per DSRM soddisfa i requisiti di complessità di dominio.|  
|VerifySafeModePassword|*N/D*|Convalidare i locale amministratore password set soddisfa computer sicurezza criteri complessità requisiti.|  
  


