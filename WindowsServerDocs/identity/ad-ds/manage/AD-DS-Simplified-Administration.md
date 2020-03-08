---
ms.assetid: f74eec9a-2485-4ee0-a0d8-cce01250a294
title: Amministrazione semplificata di Active Directory Domain Services
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 4f12b1e88414a17c8fb82a707bd4399505df4c6c
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371519"
---
# <a name="ad-ds-simplified-administration"></a>Amministrazione semplificata di Active Directory Domain Services

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono illustrate le funzionalità e i vantaggi della distribuzione e dell'amministrazione dei controller di dominio di Windows Server 2012 e le differenze tra la distribuzione del controller di dominio del sistema operativo precedente e la nuova implementazione di Windows Server 2012.  
  
In Windows Server 2012 è stata introdotta la nuova generazione di Active Directory Domain Services amministrazione semplificata ed è stata la nuova visione radicale del dominio rispetto a Windows 2000 Server. L'amministrazione semplificata di Servizi di dominio Active Directory prende le mosse dagli insegnamenti appresi in dodici anni di Active Directory per offrire un'esperienza amministrativa più supportata, flessibile e intuitiva ad architetti e amministratori. Il risultato è stata la creazione di nuove versioni delle tecnologie esistenti oltre all'estensione delle funzionalità dei componenti rilasciati in Windows Server 2008 R2.  
  
L'Amministrazione semplificata di Servizi di dominio Active Directory è una nuova forma della distribuzione dei domini.  
  
- La distribuzione dei ruoli di Servizi di dominio Active Directory fa ora parte della nuova architettura di Server Manager e consente l'installazione remota  
- Il motore di distribuzione e configurazione di Servizi di dominio Active Directory è ora Windows PowerShell, anche quando si usa la nuova Configurazione guidata Servizi di dominio Active Directory  
- L'estensione dello schema, la preparazione della foresta e la preparazione del dominio vengono eseguite automaticamente durante l'innalzamento di livello del controller di dominio e non richiedono più attività separate su speciali server come il master schema  
- L'innalzamento di livello include ora il controllo dei prerequisiti che convalida la disponibilità di foreste e domini, riducendo la possibilità di innalzamenti di livello non riusciti  
- Il modulo Active Directory per Windows PowerShell ora include i cmdlet per la gestione della topologia di replica, il controllo dinamico degli accessi e altre operazioni  
- Il livello di funzionalità della foresta di Windows Server 2012 non implementa nuove caratteristiche e il livello di funzionalità del dominio è richiesto solo per un subset di nuove caratteristiche Kerberos, sollevando gli amministratori dalla frequente necessità di un ambiente di controller di dominio omogeneo  
- È stato aggiunto il supporto completo per i controller di dominio virtualizzati, per includere la distribuzione automatizzata e la protezione del ripristino dello stato precedente  
   - Per ulteriori informazioni sui controller di dominio virtualizzati, vedere [Introduzione a servizi di dominio Active Directory e 40 #; AD DS & #41; Virtualizzazione & #40; Livello 100 & #41;](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).

Sono stati apportati anche diversi miglioramenti all'amministrazione e alla manutenzione:  

- Il Centro di amministrazione di Active Directory include il Cestino di Active Directory grafico, la gestione dei criteri password granulari e il Visualizzatore della cronologia di Windows PowerShell
- Il nuovo Server Manager ha interfacce specifiche di Servizi di dominio Active Directory per il monitoraggio delle prestazioni, l'analisi delle procedure consigliate, i servizi critici e i registri eventi  
- Gli account del servizio gestiti del gruppo supportano più computer con le stesse entità di sicurezza  
- Miglioramenti nel rilascio e nel monitoraggio degli identificatori relativi (RID) per una migliore gestibilità nei domini di Active Directory maturi  

Servizi di dominio Active Directory trae vantaggio da altre nuove funzionalità incluse in Windows Server 2012, ad esempio:  

- Gruppo NIC e bridging dei data center  
- Sicurezza DNS e disponibilità più rapida della zona integrata in Active Directory dopo il bootstrap  
- Miglioramenti all'affidabilità e alla scalabilità di Hyper-V  
- Sblocco di rete via BitLocker  
- Altri moduli di amministrazione dei componenti di Windows PowerShell  

## <a name="adprep-integration"></a>Integrazione di ADPREP

L'estensione dello schema della foresta di Active Directory e la preparazione del dominio sono ora integrate nel processo di configurazione del controller di dominio. Se si innalza di livello un nuovo controller di dominio in una foresta esistente, il processo individua lo stato dell'aggiornamento e le fasi di estensione dello schema e di preparazione del dominio vengono eseguite automaticamente. L'utente che installa il primo controller di dominio di Windows Server 2012, tuttavia, deve far parte dei gruppi Enterprise Admins e Schema Admins o fornire valide credenziali alternative.  
  
Adprep.exe rimane sul DVD per la foresta separata e la preparazione del dominio. La versione dello strumento incluso in Windows Server 2012 è compatibile con le versioni precedenti Windows Server 2008 x64 e Windows Server 2008 R2. Adprep.exe supporta anche forestprep e domainprep remoti, proprio come gli strumenti di configurazione dei controller di dominio basati su ADDSDeployment.  
  
Per informazioni su Adprep e preparazione della foresta precedente sistema operativo, vedere [esecuzione di Adprep (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd464018(WS.10).aspx).  

## <a name="server-manager-ad-ds-integration"></a>Integrazione con Servizi di dominio Active Directory di Server Manager

![amministrazione semplificata](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_Dashboard.png)  
  
Server Manager funge da hub per le attività di gestione del server. L'aspetto è simile a quello di un dashboard in cui vengono periodicamente aggiornate le visualizzazioni dei ruoli installati e dei gruppi di server remoti. Server Manager consente la gestione centralizzata dei server locali e remoti, senza bisogno di accedere alla console.  
  
Servizi di dominio Active Directory è uno di questi ruoli hub; eseguendo Server Manager in un controller di dominio o gli strumenti di amministrazione Server remoto in un Windows 8, vedrai principali problemi recenti nei controller di dominio nella foresta.  
  
Le viste disponibili sono le seguenti:  
  
- Disponibilità dei server  
- Avvisi di monitoraggio delle prestazioni relativi a un utilizzo elevato della memoria e della CPU  
- Stato dei servizi di Windows specifici di Servizi di dominio Active Directory  
- Voci recenti di avvisi e di errori relativi ai servizi directory nel registro eventi  
- Analisi delle procedure consigliate di un controller di dominio a confronto con un set di regole consigliate da Microsoft  

## <a name="active-directory-administrative-center-recycle-bin"></a>Cestino del Centro di amministrazione di Active Directory

![amministrazione semplificata](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_ADAC.png)  
  
In Windows Server 2008 R2 è stato introdotto il Cestino di Active Directory, che recupera gli oggetti di Active Directory eliminati senza ripristinarli dal backup, riavviare il servizio Servizi di dominio Active Directory o riavviare i controller di dominio.  
  
Windows Server 2012 migliora le funzionalità di ripristino basate su Windows PowerShell con una nuova interfaccia grafica nel Centro di amministrazione di Active Directory. Ciò consente agli amministratori di abilitare il Cestino e di individuare o ripristinare gli oggetti eliminati nei contesti del dominio della foresta senza eseguire direttamente i cmdlet di Windows PowerShell. Il Centro di amministrazione di Active Directory e il Cestino di Active Directory tuttavia usano Windows PowerShell in background e pertanto gli script e le procedure precedenti sono ancora utili.  
  
Per informazioni su Active Directory [Cestino, vedere Active Directory Guida dettagliata al Cestino (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd392261(WS.10).aspx).  
  
## <a name="active-directory-administrative-center-fine-grained-password-policy"></a>Criteri granulari per le password del Centro di amministrazione di Active Directory

![amministrazione semplificata](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_FGPP.png)  
  
In Windows Server 2008 sono stati introdotti i criteri granulari per le password, che consentono agli amministratori di configurare più criteri di blocco account e password per ogni dominio. Questo offre una soluzione flessibile per applicare regole per le password più o meno restrittive, basate su utenti e gruppi. Non è disponibile un'interfaccia di gestione e gli amministratori devono configurarli con Ldp.exe o Adsiedit.msc. In Windows Server 2008 R2 è stato introdotto il modulo Active Directory per Windows PowerShell, che offre agli amministratori un'interfaccia della riga di comando per i criteri granulari per le password.  
  
In Windows Server 2012 è disponibile un'interfaccia grafica per i criteri granulari per le password. Il Centro di amministrazione di Active Directory è il cuore di questo nuovo dialogo, che rende disponibile a tutti gli amministratori la gestione semplificata dei criteri granulari per le password.  
  
Per altre informazioni sulla gestione delle password, vedere [Guida dettagliata alla configurazione dei criteri specifici per le password e il blocco degli account per utenti di Servizi di dominio Active Directory (Windows Server 2008 R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx).  
  
## <a name="active-directory-administrative-center-windows-powershell-history-viewer"></a>Visualizzatore della cronologia di Windows PowerShell del Centro di amministrazione di Active Directory

![amministrazione semplificata](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_HistoryViewer.png)  
  
In Windows Server 2008 R2 è stato introdotto il Centro di amministrazione di Active Directory, che ha sostituito il precedente snap-in Utenti e computer di Active Directory creato in Windows 2000. Il Centro di amministrazione di Active Directory crea un'interfaccia amministrativa grafica per l'allora nuovo modulo di Active Directory per Windows PowerShell.  
  
Anche se il modulo di Active Directory contiene oltre cento cmdlet, la curva di apprendimento per un amministratore può essere ripida. Poiché Windows PowerShell si integra profondamente con la strategia di amministrazione di Windows, il Centro di amministrazione di Active Directory include ora un visualizzatore che consente di vedere l'esecuzione dei cmdlet nell'interfaccia grafica. È possibile cercare, copiare, cancellare la cronologia e aggiungere note con una semplice interfaccia. Lo scopo è consentire a un amministratore di usare l'interfaccia grafica per creare e modificare gli oggetti e quindi rivederli nel visualizzatore della cronologia per saperne di più sullo script di Windows PowerShell e modificare gli esempi.  

## <a name="ad-replication-windows-powershell"></a>Windows PowerShell con replica di Active Directory

![amministrazione semplificata](media/AD-DS-Simplified-Administration/ADDS_PSNewADReplSite.png)  
  
Windows Server 2012 aggiunge altri cmdlet di replica di Active Directory al modulo Windows PowerShell di Active Directory. Questi consentono la configurazione di siti, subnet, connessioni, collegamenti di sito e bridge nuovi o esistenti. Restituiscono anche informazioni sui metadati di replica di Active Directory, sullo stato della replica, sull'accodamento e sui vettori di versione di aggiornamento. L'introduzione dei cmdlet di replica, combinati con la distribuzione e gli altri cmdlet di Servizi di dominio Active Directory esistenti, consente di amministrare una foresta solo con Windows PowerShell. Si creano così nuove opportunità per gli amministratori che desiderano effettuare il provisioning e gestire Windows Server 2012 senza interfaccia grafica, riducendo in tal modo la superficie di attacco e i requisiti di manutenzione del sistema operativo. Questo è importante soprattutto quando si distribuiscono server in reti con sicurezza elevata, come SIPR (Secret Internet Protocol Router) e le reti perimetrali aziendali.  
  
Per ulteriori informazioni sulla topologia del sito di Active Directory e replica, vedere il [documentazione tecnica su Windows Server](https://technet.microsoft.com/library/cc739127(WS.10).aspx).  

## <a name="rid-management-and-issuance-improvements"></a>Miglioramenti della gestione e del rilascio di RID

In Active Directory di Windows 2000 è stato introdotto il master RID, che rilascia pool di identificatori relativi ai controller di dominio, per creare ID di sicurezza (SID) di domini trusted di sicurezza, come utenti, gruppi e computer.  Per impostazione predefinita, questo spazio RID globale è limitato a 2<sup>30</sup> (o 1.073.741.823) SID totali creati in un dominio. I SID non possono essere restituiti al pool o essere nuovamente rilasciati. Con il tempo, è possibile che un dominio di grandi dimensioni inizi a essere a corto di RID o che degli incidenti portino a un consumo non necessario di RID e a un eventuale esaurimento.  
  
In Windows Server 2012 sono stati risolti diversi problemi di rilascio e gestione dei RID individuati dai clienti e dal supporto tecnico Microsoft con lo sviluppo di Servizi di dominio Active Directory dopo la creazione dei primi domini di Active Directory nel 1999. tra cui:  

- Vengono scritti periodicamente avvisi sull'utilizzo di RID nel registro eventi  
- Vengono registrati eventi quando un amministratore invalida un pool di RID  
- Viene ora applicato un tetto massimo alla dimensione del blocco di RID in base ai criteri RID  
- Vengono ora applicati e registrati limiti massimi di RID artificiali quando lo spazio RID globale è scarso, consentendo a un amministratore di intervenire prima che lo spazio si esaurisca
- Lo spazio RID globale ora può essere incrementato di un bit, raddoppiando la dimensione a 2<sup>31</sup> (2.147.483.648 SID)  

Per altre informazioni sui RID e sul master RID, vedere [Come funzionano gli ID di sicurezza](https://technet.microsoft.com/library/cc778824(WS.10).aspx).  
  
## <a name="ad-ds-role-deployment-and-management-architecture"></a>Architettura di distribuzione e gestione del ruolo di Servizi di dominio Active Directory

Server Manager e Windows PowerShell ADDSDeployment si basano sui seguenti assembly principali per la funzionalità quando si distribuisce o gestisce il ruolo di Servizi di dominio Active Directory:  

- Microsoft.ADroles.Aspects.dll  
- Microsoft.ADroles.Instrumentation.dll  
- Microsoft.ADRoles.ServerManager.Common.dll  
- Microsoft.ADRoles.UI.Common.dll  
- Microsoft.DirectoryServices.Deployment.Types.dll  
- Microsoft.DirectoryServices.ServerManager.dll  
- Addsdeployment.psm1  
- Addsdeployment.psd1  

Entrambi si basano su Windows PowerShell e sul comando di chiamata remota per l'installazione e la configurazione remote del ruolo.  

![amministrazione semplificata](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_DepDLLs.png)  

Windows Server 2012 inoltre effettua il refactoring di diverse operazioni di innalzamento di livello esternamente a LSASS.EXE, come parte di:  

- Servizio Ruolo server DS (DsRoleSvc)  
- DSRoleSvc.dll (caricata dal servizio DsRoleSvc)  

Questo servizio deve essere presente e in esecuzione per innalzare di livello, abbassare di livello o clonare i controller di dominio virtuali. Per impostazione predefinita, l'installazione del ruolo di Servizi di dominio Active Directory aggiunge questo servizio e imposta un tipo di avvio manuale. Non disabilitare questo servizio.  

## <a name="adprep-and-prerequisite-checking-architecture"></a>ADPrep e architettura di controllo dei prerequisiti

Non è più necessario eseguire Adprep sul master schema. Può essere eseguito in modalità remota da un computer che esegue Windows Server 2008 x64 o versione successiva.  
  
> [!NOTE]  
> Adprep usa LDAP per importare i file Schxx.ldf e non si riconnette automaticamente se la connessione al master schema viene persa durante l'importazione. Durante il processo di importazione, il master schema viene impostato in una modalità specifica e la riconnessione automatica viene disabilitata perché, se LDAP si riconnette dopo la perdita della connessione, la connessione ristabilita non sarà nella modalità specifica. In tal caso, lo schema non verrà aggiornato correttamente.  
  
Il controllo dei prerequisiti verifica che determinate condizioni siano vere. Queste condizioni sono obbligatorie per installare correttamente Servizi di dominio Active Directory. Se alcune condizioni obbligatorie non sono vere, è possibile risolverle prima di continuare l'installazione. La verifica inoltre rileva se una foresta o un dominio non è ancora pronto e il codice di distribuzione di Adprep viene eseguito automaticamente.  

### <a name="adprep-executables-dlls-ldfs-files"></a>Eseguibili ADPrep, DLL, LDF, file

- ADprep.dll  
- Ldifde.dll  
- Csvde.dll  
- Sch14.ldf - Sch56.ldf  
- Schupgrade.cat  
- *dcpromo.csv  

Del codice di preparazione di Active Directory in precedenza incluso in ADprep.exe viene effettuato il refactoring in adprep.dll. Questo consente sia ad ADPrep.exe che al modulo ADDSDeployment di Windows PowerShell di usare la libreria per le stesse attività e di avere le stesse funzionalità. Adprep.exe è incluso nei supporti di memorizzazione, ma i processi automatizzati non lo chiamano direttamente. Solo un amministratore lo esegue manualmente. Può essere eseguito solo su Windows Server 2008 x64 e sistemi operativi successivi. Anche ldifde.exe e csvde.exe hanno versioni di cui è stato effettuato il rectoring come DLL che vengono caricate dal processo di preparazione. L'estensione dello schema usa tuttavia file LDF con verifica della firma, come nelle versioni precedenti del sistema operativo.  
  
![amministrazione semplificata](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_AdprepDLLs.png)  
  
> [!IMPORTANT]  
> Per Windows Server 2012 non è disponibile lo strumento Adprep32.exe a 32 bit. È necessario disporre di almeno un computer Windows Server 2008 x64, Windows Server 2008 R2 o Windows Server 2012, in esecuzione come controller di dominio, server membro o in un gruppo di lavoro, per preparare la foresta e il dominio. Adprep.exe non viene eseguito su Windows Server 2003 x64.  
  
## <a name="BKMK_PrereuisiteChecking"></a>Controllo dei prerequisiti

Il sistema di controllo dei prerequisiti predefinito del codice gestito di Windows PowerShell ADDSDeployment funziona in modalità diverse a seconda dell'operazione. Nelle tabelle seguenti viene descritto ogni test e viene specificato quando viene usato e come e che cosa convalida. Queste tabelle possono essere utili se si verificano problemi a causa dei quali la convalida non riesce e l'errore non è sufficiente per risolvere il problema.  
  
Questi test vengono registrati nel canale del registro eventi operativi **DirectoryServices-Deployment** sotto la categoria attività **Base**, sempre come ID evento **103**.  
  
### <a name="prerequisite-windows-powershell"></a>Windows PowerShell per i prerequisiti

Sono disponibili cmdlet ADDSDeployment di Windows PowerShell per tutti i cmdlet di distribuzione dei controller di dominio. Hanno all'incirca gli stessi argomenti dei cmdlet associati.  

- Test-ADDSDomainControllerInstallation  
- Test-ADDSDomainControllerUninstallation  
- Test-ADDSDomainInstallation  
- Test-ADDSForestInstallation  
- Test-ADDSReadOnlyDomainControllerAccountCreation  

In genere non è necessario eseguire questi cmdlet. Per impostazione predefinita, vengono già eseguiti automaticamente con i cmdlet di distribuzione.  

#### <a name="BKMK_ADDSInstallPrerequisiteTests"></a>Test dei prerequisiti

||||  
|-|-|-|  
|Nome test|Protocolli<br /><br />used|Spiegazione e note|  
|VerifyAdminTrusted<br /><br />ForDelegationProvider|LDAP|Convalida che si dispone del privilegio "Impostazione account computer ed utente a tipo trusted per la delega" (SeEnableDelegationPrivilege) sul controller di dominio partner esistente. È necessario l'accesso all'attributo costruito tokenGroups.<br /><br />Non è usato quando si contattano controller di dominio di Windows Server 2003. È necessario confermare manualmente questo privilegio prima dell'innalzamento di livello|  
|VerifyADPrep<br /><br />Prerequisiti (foresta)|LDAP|Individua e contatta il master schema con gli attributi namingContexts di rootDSE e l'attributo fsmoRoleOwner del contesto dei nomi dello schema. Determina le operazioni preliminari (forestprep, domainprep o rodcprep) necessarie per l'installazione di Servizi di dominio Active Directory. Convalida che lo schema objectVersion è previsto e se richiede un'ulteriore estensione.|  
|VerifyADPrep<br /><br />Prerequisiti (dominio e controller di dominio di sola lettura)|LDAP|Individua e contatta il master infrastrutture con l'attributo namingContexts di rootDSE e l'attributo fsmoRoleOwner del contenitore di infrastrutture. Se viene installato un controller di dominio di sola lettura, questo test individua il master per la denominazione dei domini e verifica che sia online.|  
|CheckGroup<br /><br />Appartenenze|LDAP,<br /><br />RPC su SMB (LSARPC)|Convalida che l'utente è membro del gruppo Domain Admins o Enterprise Admins, a seconda dell'operazione (DA per l'aggiunta o l'abbassamento di livello di un controller di dominio, EA per l'aggiunta o la rimozione di un dominio).|  
|CheckForestPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC su SMB (LSARPC)|Convalida che l'utente è membro dei gruppi Schema Admins ed Enterprise Admins e disponga del privilegio Gestione registri di controllo ed eventi sicurezza (SesScurityPrivilege) sui controller di dominio esistenti.|  
|CheckDomainPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC su SMB (LSARPC)|Convalida che l'utente è membro del gruppo Domain Admins e disponga del privilegio Gestione registri di controllo ed eventi sicurezza (SesScurityPrivilege) sui controller di dominio esistenti.|  
|CheckRODCPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC su SMB (LSARPC)|Convalida che l'utente è membro del gruppo Enterprise Admins e disponga del privilegio Gestione registri di controllo ed eventi sicurezza (SesScurityPrivilege) sui controller di dominio esistenti.|  
|VerifyInitSync<br /><br />AfterReboot|LDAP|Convalida che il master schema è stato replicato almeno una volta dopo il riavvio impostando un valore fittizio per l'attributo becomeSchemaMaster di rootDSE.|  
|VerifySFUHotFix<br /><br />Applicato|LDAP|Convalida che lo schema della foresta esistente non contiene l'estensione del problema noto SFU2 per l'attributo UID con OID 1.2.840.113556.1.4.7000.187.102.<br /><br />([https://support.microsoft.com/kb/821732](https://support.microsoft.com/kb/821732))|  
|VerifyExchange<br /><br />SchemaFixed|LDAP, WMI, DCOM, RPC|Convalida che lo schema della foresta esistente non contiene ancora le estensioni di Exchange 2000 per il problema ms-Exch-Assistant-Name, ms-Exch-LabeledURI e ms-Exch-House-Identifier ([https://support.microsoft.com/kb/314649](https://support.microsoft.com/kb/314649))|  
|VerifyWin2KSchema<br /><br />Coerenza|LDAP|Convalida che lo schema della foresta esistente ha attributi e classi principali coerenti (non erroneamente modificati da terze parti).|  
|DCPromo|DRSR su RPC,<br /><br />LDAP,<br /><br />DNS<br /><br />RPC su SMB (SAMR)|Convalida la sintassi della riga di comando passata al codice di innalzamento di livello e testa l'innalzamento di livello. Convalida che la foresta o il dominio non esiste già se ne sta creando uno nuovo.|  
|VerifyOutbound<br /><br />ReplicationEnabled|LDAP, DRSR su SMB, RPC su SMB (LSARPC)|Convalida che la replica in uscita del controller di dominio esistente specificato come partner di replica è abilitata controllando l'attributo delle opzioni dell'oggetto Impostazioni NTDS per NTDSDSA_OPT_DISABLE_OUTBOUND_REPL (0x00000004).|  
|VerifyMachineAdmin<br /><br />Password|DRSR su RPC,<br /><br />LDAP,<br /><br />DNS<br /><br />RPC su SMB (SAMR)|Convalida che la password della modalità provvisoria impostata per DSRM soddisfa i requisiti di complessità del dominio.|  
|VerifySafeModePassword|*N/D*|Convalida che la password di amministratore locale impostata soddisfa i requisiti di complessità dei criteri di sicurezza del computer.|  
