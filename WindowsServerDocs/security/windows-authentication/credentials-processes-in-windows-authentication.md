---
title: Processi di credenziali in autenticazione di Windows
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48c60816-fb8b-447c-9c8e-800c2e05b14f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 52d50a1bb6bfbe9d35146f362a2a5184df0a6504
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="credentials-processes-in-windows-authentication"></a>Processi di credenziali in autenticazione di Windows

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento di riferimento per i professionisti IT descrive le modalità di elaborazione delle credenziali di autenticazione di Windows.

Gestione credenziali di Windows è il processo mediante il quale il sistema operativo riceve le credenziali del servizio o l'utente e protegge le informazioni per la presentazione future alla destinazione di autenticazione. Nel caso di un computer aggiunto al dominio, la destinazione di autenticazione è il controller di dominio. Le credenziali usate nell'autenticazione sono documenti digitali che associa l'identità dell'utente in una forma di una verifica dell'autenticità, ad esempio un certificato, una password o un PIN.

Per impostazione predefinita, le credenziali di Windows vengono convalidate nel database Gestione account di sicurezza (SAM) nel computer locale o in Active Directory in un computer aggiunto al dominio, tramite il servizio Winlogon. Le credenziali vengono raccolte tramite l'input dell'utente nell'interfaccia utente di accesso o a livello di codice tramite l'interfaccia di programmazione applicazioni (API) devono essere presentati al computer di destinazione che esegue l'autenticazione.

Informazioni di sicurezza locali vengono archiviate nel Registro di sistema **HKEY_LOCAL_MACHINE\SECURITY**. Informazioni archiviate includono le impostazioni di criteri, i valori di sicurezza predefiniti e informazioni sull'account, ad esempio le credenziali di accesso memorizzato nella cache. Una copia del database SAM viene memorizzata in questo caso, anche se è protetto.

Il diagramma seguente mostra i componenti necessari e i percorsi che le credenziali richiedere attraverso il sistema per autenticare l'utente o il processo per l'accesso.

![Diagramma che mostra i componenti necessari e i percorsi che le credenziali richiedere attraverso il sistema per autenticare l'utente o il processo per l'accesso.](../media/credentials-processes-in-windows-authentication/AuthN_LSA_Architecture_Client.gif)

La tabella seguente descrive ogni componente che gestisce le credenziali nel processo di autenticazione nel punto di accesso.

**Componenti di autenticazione per tutti i sistemi**

|Componente|Descrizione|
|-------|--------|
|Accesso utente|Winlogon.exe è il file eseguibile responsabile della gestione delle interazioni dell'utente protetto. Il servizio Winlogon avvia il processo di accesso per i sistemi operativi Windows passando le credenziali raccolte dall'azione dell'utente sul desktop sicuro (dell'interfaccia utente di accesso) per la sicurezza autorità locale (LSA) tramite Secur32.dll.|
|Accesso all'applicazione|L'applicazione o servizio accessi che non richiedono l'accesso interattivo. La maggior parte dei processi avviati dall'utente di eseguire in modalità utente utilizzando Secur32.dll mentre avviati all'avvio, ad esempio servizi, processi eseguiti in modalità kernel utilizzando Ksecdd.sys.<br /><br />Per ulteriori informazioni su modalità utente e la modalità kernel, vedere le applicazioni e modalità utente o i servizi e modalità Kernel in questo argomento.|
|Secur32.dll|I provider di autenticazione più elaborate sulla base dell'autenticazione.|
|Lsasrv.dll|Il servizio Server di LSA, che impone i criteri di sicurezza e funge da Gestione pacchetto di protezione per LSA. LSA contiene la funzione Negotiate, che seleziona il protocollo NTLM o Kerberos dopo avere stabilito il protocollo è abbia esito positivo.|
|Security Support Provider|Un insieme di provider che può richiamare singolarmente uno o più protocolli di autenticazione. Con ogni versione del sistema operativo Windows possa modificare il set predefinito di provider e provider personalizzati possono essere scritti.|
|Netlogon.dll|I servizi che esegue il servizio Accesso rete sono i seguenti:<br /><br />-Mantiene canale sicuro del computer (non deve essere confuso con Schannel) a un controller di dominio.<br />-Passa le credenziali dell'utente tramite un canale sicuro al controller di dominio e restituisce gli identificatori di protezione dominio (SID) e i diritti utente per l'utente.<br />-Pubblica record di risorse servizio nel sistema DNS (Domain Name) e Usa DNS per risolvere i nomi per gli indirizzi IP (Internet Protocol) di controller di dominio.<br />-Implementa il protocollo di replica in base alle chiamate di procedura remota (RPC) per la sincronizzazione dei controller di dominio primario (PDC) e il controller di dominio di backup (BDC).|
|Samsrv.dll|L'account di protezione Manager (SAM), che archivia gli account di protezione locale, applica i criteri archiviati localmente e supporta le API.|
|Registro di sistema|Il Registro di sistema contiene una copia del database SAM, impostazioni di criteri di protezione locali, i valori di sicurezza predefiniti e informazioni sull'account che è possibile accedere al sistema solo.|

In questo argomento contiene le sezioni seguenti:

-   [Input di credenziali per l'accesso utente](#BKMK_CrentialInputForUserLogon)

-   [Input di credenziali per l'applicazione e l'accesso al servizio](#BKMK_CredentialInputForApplicationAndServiceLogon)

-   [Autorità di protezione locale](#BKMK_LSA)

-   [La convalida e credenziali memorizzate nella cache](#BKMK_CachedCredentialsAndValidation)

-   [Archiviazione delle credenziali e convalida](#BKMK_CredentialStorageAndValidation)

-   [Database di gestione degli account di sicurezza](#BKMK_SAM)

-   [I domini locali e dei domini trusted](#BKMK_LocalDomainsAndTrustedDomains)

-   [Certificati di autenticazione di Windows](#BKMK_CertificatesInWindowsAuthentication)

## <a name="BKMK_CrentialInputForUserLogon"></a>Input di credenziali per l'accesso utente
In Windows Server 2008 e Windows Vista, l'architettura Graphical Identification and Authentication (GINA) è stato sostituito con un modello di provider di credenziali, che è stato possibile enumerare i diversi tipi di accesso tramite l'utilizzo di riquadri di accesso. Entrambi i modelli sono descritte di seguito.

**Architettura GINA**

L'architettura Graphical Identification and Authentication (GINA) si applica ai sistemi operativi Windows Server 2003, Microsoft Windows 2000 Server, Windows XP e Windows 2000 Professional. In questi sistemi, ogni sessione di accesso interattivo crea un'istanza separata del servizio Winlogon. L'architettura GINA viene caricato nello spazio del processo utilizzato da Winlogon, riceve e le credenziali, viene elaborato e le chiamate alle interfacce di autenticazione tramite LSALogonUser.

Le istanze di Winlogon per un accesso interattivo eseguiti nella sessione 0. Servizi di sistema di host sessione 0 e gli altri processi critici, incluso il processo di autorità di sicurezza locale (LSA).

Il diagramma seguente illustra il processo di credenziali per Windows Server 2003, Microsoft Windows 2000 Server, Windows XP e Microsoft Windows 2000 Professional.

![Diagramma che illustra il processo di credenziali per Windows Server 2003, Microsoft Windows 2000 Server, Windows XP e Microsoft Windows 2000 Professional](../media/credentials-processes-in-windows-authentication/AuthN_GINA_Architecture.gif)

**Architettura del provider di credenziali**

Si applica l'architettura del provider di credenziali per le versioni indicate nel **si applica a** elenco all'inizio di questo argomento. In questi sistemi, l'architettura di input di credenziali modificate a una progettazione extensible tramite provider di credenziali. These providers are represented by the different logon tiles on the secure desktop that permit any number of logon scenarios - different accounts for the same user and different authentication methods, such as password, smart card, and biometrics.

Con l'architettura del provider di credenziali, Winlogon avvia sempre dell'interfaccia utente di accesso dopo aver ricevuto un evento di sequenza di attenzione sicuro. Interfaccia utente di accesso esegue una query su ogni provider di credenziali per il numero di tipi di credenziali diverso che il provider è configurato per enumerare. Provider di credenziali ha la possibilità di specificare uno di questi riquadri come predefinito. Dopo che i riquadri enumerati tutti i provider, interfaccia utente di accesso li visualizza all'utente. L'utente interagisce con un riquadro per fornire le credenziali. Interfaccia utente di accesso invia le credenziali per l'autenticazione.

Provider di credenziali non sono meccanismi di imposizione. Vengono utilizzati per raccogliere e serializzare le credenziali. I pacchetti di autenticazione e l'autorità di sicurezza locale di applicare la protezione.

Provider di credenziali sono registrati nel computer e sono responsabili per le operazioni seguenti:

-   Che descrive le informazioni sulle credenziali richieste per l'autenticazione.

-   Gestione delle comunicazioni e la logica con autorità di autenticazione esterno.

-   Le credenziali di creazione di pacchetti per interattivo e l'accesso alla rete.

Le credenziali di creazione di pacchetti per interattivo e l'accesso alla rete include il processo di serializzazione. Per serializzare le credenziali più riquadri di accesso possono essere visualizzati su interfaccia utente di accesso. Therefore, your organization can control the logon display such as users, target systems for logon, pre-logon access to the network and workstation lock/unlock policies - through the use of customized credential providers. Più provider di credenziali possono coesistere nello stesso computer.

È possibile sviluppare Single sign-on (SSO) i provider di un provider di credenziali standard o da un Provider di Pre-Logon-Access.

Ogni versione di Windows contiene il provider di credenziali di uno predefinito e uno predefinito Pre-Logon-Access Provider (PLAP), noto anche come provider di SSO. Il provider SSO consente agli utenti di effettuare una connessione a una rete prima dell'accesso al computer locale. Quando viene implementato questo provider, il provider enumera i riquadri sull'interfaccia utente di accesso.

Un provider di SSO deve essere usato negli scenari seguenti:

-   **Accesso computer e di autenticazione di rete vengono gestiti dal provider di credenziali diverso.** Varianti di questo scenario includono:

    -   Un utente ha la possibilità di connettersi a una rete, ad esempio la connessione a una rete privata virtuale (VPN), prima di accedere al computer ma non è necessario per questa connessione.

    -   Autenticazione di rete è necessario recuperare le informazioni utilizzate durante l'autenticazione interattiva nel computer locale.

    -   Più le autenticazioni di rete sono seguite da uno degli altri scenari. Ad esempio, un utente esegue l'autenticazione a un provider di servizi Internet (ISP), autentica a una VPN e quindi Usa le credenziali dell'account utente per l'accesso in locale.

    -   Credenziali memorizzate nella cache sono disabilitate ed è necessaria una connessione di servizi di accesso remoto tramite VPN prima dell'accesso locale per autenticare l'utente.

    -   Un utente di dominio non dispone di un account locale configurato in un computer aggiunto al dominio e deve stabilire una connessione di servizi di accesso remoto attraverso la connessione VPN prima di completare l'accesso interattivo.

-   **Accesso computer e di autenticazione di rete vengono gestiti dallo stesso provider di credenziali.** In questo scenario, l'utente è richiesto per la connessione alla rete prima di accedere al computer.

**Enumerazione di riquadri di accesso**

Il provider di credenziali enumera i riquadri di accesso nei seguenti casi:

-   Per tali sistemi operativi indicate nel **si applica a** elenco all'inizio di questo argomento.

-   Il provider di credenziali enumera i riquadri per l'accesso a una workstation. In genere, il provider di credenziali serializza credenziali per l'autenticazione per l'autorità di sicurezza locale. Questo processo consente di visualizzare i riquadri specifici per ogni utente e ai sistemi di destinazione di ciascun utente.

-   L'architettura di accesso e dell'autenticazione consente di utilizzare riquadri enumerati tramite il provider di credenziali per sbloccare una workstation. In genere, l'utente attualmente connesso è il riquadro predefinito, ma se più di un utente è connesso, numerosi riquadri vengono visualizzati.

-   Il provider di credenziali enumera i riquadri in risposta a una richiesta utente di modificare la password o altre informazioni private, ad esempio un PIN. In genere, l'utente attualmente connesso è il riquadro predefinito; Tuttavia, se più di un utente è connesso, numerosi riquadri vengono visualizzati.

-   Il provider di credenziali enumera i riquadri in base alle credenziali serializzate da utilizzare per l'autenticazione nei computer remoti. Credenziali dell'interfaccia utente non utilizzano la stessa istanza del provider come interfaccia utente di accesso, sbloccare una Workstation o modificare la Password. Di conseguenza, le informazioni sullo stato non vengono mantenute nel provider tra le istanze dell'interfaccia utente delle credenziali. Questa struttura restituisce un solo riquadro per ogni accesso al computer remoto, presupponendo che le credenziali sono stati correttamente serializzate. Questo scenario viene inoltre utilizzato in controllo Account utente (UAC), che consente di impedire modifiche non autorizzate a un computer per chiedere conferma all'utente un'autorizzazione o una password amministratore prima di consentire azioni che potrebbero influire sul funzionamento del computer o che potrebbero modificare le impostazioni che influiscono su altri utenti del computer.

Il diagramma seguente viene illustrato il processo di credenziali per i sistemi operativi indicate nel **si applica a** elenco all'inizio di questo argomento.

![Diagramma che illustra il processo di credenziali per i sistemi operativi indicate di * * si applica a * * elenco all'inizio di questo argomento](../media/credentials-processes-in-windows-authentication/AuthN_CredMan_CredProv.gif)

## <a name="BKMK_CredentialInputForApplicationAndServiceLogon"></a>Input di credenziali per l'applicazione e l'accesso al servizio
Autenticazione di Windows è progettata per gestire le credenziali per applicazioni o servizi che non richiedono l'interazione dell'utente. Applicazioni in modalità utente sono limitate in termini di quali risorse di sistema hanno accesso, mentre i servizi possono avere accesso illimitato alla memoria di sistema e i dispositivi esterni.

Servizi di sistema e le applicazioni di livello del trasporto accedono tramite Security supporto Provider Interface (SSPI) in Windows, che offre funzioni per enumerare i pacchetti di sicurezza disponibili in un sistema, la selezione di un pacchetto e l'utilizzo di tale pacchetto per ottenere una connessione autenticata Security Support Provider (SSP).

Quando viene autenticata una connessione client/server:

-   L'applicazione sul lato client della connessione invia le credenziali al server utilizzando la funzione SSPI `InitializeSecurityContext (General)`.

-   L'applicazione sul lato server della connessione risponde con la funzione SSPI `AcceptSecurityContext (General)`.

-   Le funzioni SSPI `InitializeSecurityContext (General)`e `AcceptSecurityContext (General)`vengono ripetuti fino a quando non sono stati scambiati tutti i messaggi di autenticazione necessarie per l'autenticazione avrà esito negativo o esito positivo.

-   Dopo la connessione è stata autenticata, LSA sul server utilizza le informazioni dal client per creare il contesto di sicurezza, che contiene un token di accesso.

-   Il server può quindi chiamare la funzione SSPI `ImpersonateSecurityContext`per collegare il token di accesso a un thread di rappresentazione per il servizio.

**Le applicazioni e la modalità utente**

Modalità utente in Windows è costituita da due sistemi in grado di superare le richieste dei / o ai driver in modalità kernel appropriato: il sistema di ambiente, che esegue applicazioni scritte per molti tipi diversi di sistemi operativi, e il sistema integrale, che opera funzioni specifiche del sistema per conto del sistema di ambiente.

Il sistema integrale gestisce funzioni system'specific operative per conto del sistema di ambiente e costituito da un processo di sistema di sicurezza (LSA), un servizio workstation e un servizio server. Il processo di protezione del sistema gestisce i token di sicurezza concede o Nega le autorizzazioni per accedere agli account utente in base alle autorizzazioni per le risorse, gestisce le richieste di accesso e avvia l'autenticazione di accesso e determina le risorse di sistema deve controllare il sistema operativo.

Applicazioni possono essere eseguite in modalità utente in cui eseguire l'applicazione come qualsiasi entità, inclusi nel contesto di sicurezza del sistema locale (sistema). Applicazioni possono essere eseguite in modalità kernel in cui eseguire l'applicazione nel contesto di sicurezza del sistema locale (sistema).

SSPI è disponibile tramite il modulo Secur32.dll, che è un'API utilizzata per ottenere i servizi di sicurezza integrata per l'autenticazione, integrità e riservatezza dei messaggi. Fornisce un livello di astrazione tra i protocolli a livello di applicazione e i protocolli di protezione. Poiché diverse applicazioni richiedono diversi modi per identificare o l'autenticazione di utenti e i modi diversi per la crittografia dei dati durante il trasferimento in rete, SSPI fornisce un modo per accedere a librerie di collegamento dinamico (DLL) che contengono le funzioni di crittografia e autenticazione diversi. Queste DLL sono denominate Security Support Provider (SSP).

Servizi gestiti gli account e gli account virtuali sono state introdotte in Windows Server 2008 R2 e Windows 7 per assicurare ad applicazioni cruciali, ad esempio Microsoft SQL Server e Internet Information Services (IIS), l'isolamento dei relativi account di dominio, mentre eliminando manualmente la necessità di un amministratore di amministrare il nome dell'entità servizio (SPN) e le credenziali per questi account. For more information about these features and their role in authentication, see [Managed Service Accounts Documentation for Windows 7 and Windows Server 2008 R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx) and [Group Managed Service Accounts Overview](../group-managed-service-accounts/group-managed-service-accounts-overview.md).

**Modalità kernel e servizi**

Anche se la maggior parte delle applicazioni di Windows eseguiti nel contesto di sicurezza dell'utente che li avvia, ciò non è possibile dei servizi. Molti servizi di Windows, ad esempio rete e servizi di stampa, vengono avviati dal controller del servizio quando l'utente avvia il computer. Questi servizi potrebbero essere eseguiti come sistema locale o servizio locale e potrebbero continuare a funzionare dopo l'ultimo utente si disconnette.

> [!NOTE]
> In genere, i servizi eseguiti nei contesti di sicurezza noti come sistema locale (sistema), servizio di rete o servizio locale.  Windows Server 2008 R2 è stato introdotto servizi eseguiti con un account del servizio gestito, che sono entità di dominio.

Prima di avviare un servizio, il controller del servizio accede utilizzando l'account che viene definita per il servizio e quindi presenta le credenziali del servizio per l'autenticazione da LSA. Il servizio Windows implementa un'interfaccia di programmazione utilizzabili per controllare il servizio di Gestione controllo servizi. Un servizio di Windows può essere avviato automaticamente all'avvio del sistema o manualmente con un programma di controllo del servizio. Ad esempio, quando un computer client Windows viene aggiunto un dominio, il servizio messenger sul computer si connette a un controller di dominio e apre un canale sicuro. Per ottenere una connessione autenticata, il servizio deve disporre di credenziali che considera attendibile l'autorità di sicurezza locale (LSA del computer remoto). Durante la comunicazione con altri computer nella rete, LSA utilizza le credenziali per l'account di dominio del computer locale, come tutti gli altri servizi in esecuzione nel contesto di sicurezza del sistema locale e servizio di rete. I servizi nel computer locale eseguiti come sistema in modo che le credenziali non è necessario essere presentati a LSA.

Il file Ksecdd.sys gestisce e consente di crittografare le credenziali e utilizza una chiamata di procedura locale in LSA. Il tipo di file è DRV (driver) ed è noto come in modalità kernel Security Support Provider (SSP) e, in tali versioni indicate nel **si applica a** elenco all'inizio di questo argomento, è FIPS 140-2 di livello 1 conforme.

Modalità kernel dispone dell'accesso completo alle risorse hardware e del sistema del computer. La modalità kernel arresta modalità utente servizi e applicazioni di accedere alle aree critiche del sistema operativo che deve non hanno accesso.

## <a name="BKMK_LSA"></a>Autorità di protezione locale
L'autorità di sicurezza locale (LSA) è un processo di sistema protetti che autentica e registra gli utenti al computer locale. Inoltre, LSA gestisce le informazioni su tutti gli aspetti di sicurezza locale in un computer (questi aspetti sono noti come criterio di protezione locale) e fornisce diversi servizi per la conversione tra nomi e gli identificatori di sicurezza (SID). Il processo di sistema di sicurezza, protezione Server servizio LSASS (Local Authority), tiene traccia dei criteri di sicurezza e gli account che sono attivi in un computer.

LSA convalida l'identità dell'utente in base a quale delle due seguenti entità emesso l'account dell'utente:

-   **Autorità di protezione locale.** LSA possa convalidare le informazioni utente controllando il database Gestione account di sicurezza (SAM) si trova nello stesso computer. Qualsiasi workstation o server membro può archiviare gli account utente locale e informazioni sui gruppi locali. Tuttavia, questi account possono essere utilizzati per l'accesso solo tale workstation o computer.

-   **Autorità di sicurezza per il dominio locale o per un dominio trusted.** LSA contatta l'entità che ha emesso l'account e la verifica di richieste che l'account sia valido e che la richiesta proviene da titolare del conto.

Il LSASS Local Security Authority Subsystem Service () archivia le credenziali in memoria per conto degli utenti con sessioni attive di Windows. Le credenziali archiviate consentono agli utenti di accedere facilmente alle risorse di rete, ad esempio condivisioni di file, cassette postali di Exchange Server e siti di SharePoint senza immettere di nuovo le proprie credenziali per ogni servizio remoto.

LSASS possono archiviare credenziali in più formati, inclusi:

-   Testo normale crittografato in modo reversibile

-   Ticket Kerberos (ticket di concessione ticket (TGT), i ticket di servizio)

-   Hash NT

-   Hash di LAN Manager (LM)

Se l'utente accede a Windows utilizzando una smart card, LSASS non archivia una password non crittografata, ma archivia il valore hash NT corrispondente per l'account e il testo non crittografato PIN per la smart card. Se l'attributo dell'account è abilitato per una smart card che è necessaria per l'accesso interattivo, un valore hash NT casuale viene generato automaticamente per l'account invece l'hash della password originale. L'hash della password generata automaticamente quando è impostato l'attributo non cambia.

Se un utente accede a un computer basato su Windows con una password compatibile con hash di LAN Manager (LM), questo autenticatore è presente in memoria.

L'archiviazione delle credenziali in testo normale in memoria non può essere disabilitata, anche se i provider di credenziali che le richiedono sono disabilitati.

Le credenziali archiviate sono associate direttamente le sessioni di accesso LSASS Local Security Authority Subsystem Service () che sono state avviate dopo l'ultimo riavvio e non chiuse. Ad esempio, sessioni LSA con credenziali LSA archiviate vengono create quando un utente esegue una delle operazioni seguenti:

-   Accede a una sessione locale o una sessione di Remote Desktop Protocol (RDP) nel computer

-   Esegue un'attività con la **RunAs** opzione

-   Esegue un servizio Windows attivo nel computer

-   Esegue un processo di attività o batch pianificato

-   Esegue un'attività nel computer locale utilizzando uno strumento di amministrazione remota

In alcuni casi, i segreti LSA, che sono parti segrete di dati che sono accessibili solo ai processi di account di sistema, vengono archiviati nell'unità disco rigido. Alcuni di questi segreti sono credenziali che devono essere mantenute dopo il riavvio e sono archiviati in formato crittografato nell'unità disco rigido. Le credenziali archiviate come segreti LSA possono includere:

-   Password dell'account per l'account del computer Servizi di dominio Active Directory (AD DS)

-   Password degli account per i servizi Windows configurati nel computer

-   Password degli account per le attività pianificate configurate

-   Password degli account per pool di applicazioni IIS e siti Web

-   Password per account Microsoft

Introdotto in Windows 8.1, il sistema operativo client offre protezione aggiuntiva per LSA per impedire la lettura e l'inserimento di codice da processi non protetti. Questa protezione aumenta la sicurezza per le credenziali archiviate e gestite in LSA.

Per ulteriori informazioni su queste protezioni aggiuntive, vedere [Configuring Additional LSA Protection](../credentials-protection-and-management/configuring-additional-lsa-protection.md).

## <a name="BKMK_CachedCredentialsAndValidation"></a>La convalida e credenziali memorizzate nella cache
Meccanismi di convalida si basano sulla presentazione delle credenziali al momento dell'accesso. Tuttavia, quando il computer è disconnesso da un controller di dominio e l'utente è presentare le credenziali di dominio, Windows Usa il processo di credenziali memorizzate nella cache del meccanismo di convalida.

Ogni volta che un utente accede a un dominio, Windows memorizza nella cache le credenziali fornite e li archivia nell'hive del Registro di sistema protezione del sistema operativo.

Con le credenziali memorizzate nella cache, l'utente può accedere a un membro del dominio senza connettersi a un controller di dominio all'interno del dominio.


## <a name="BKMK_CredentialStorageAndValidation"></a>Archiviazione delle credenziali e convalida
Non è sempre consigliabile utilizzare un insieme di credenziali per l'accesso alle risorse diverse. Ad esempio, un amministratore può desidera utilizzare amministrativa anziché le credenziali dell'utente quando si accede a un server remoto. Analogamente, se un utente accede a risorse esterne, ad esempio un conto bancario, egli può utilizzare solo credenziali diverse da quelle le credenziali di dominio. Le sezioni seguenti descrivono le differenze nella gestione delle credenziali tra le versioni correnti dei sistemi operativi Windows e i sistemi operativi Windows Vista e Windows XP.

### <a name="remote-logon-credential-processes"></a>Processi di credenziali di accesso remoto
Remote Desktop Protocol (RDP) gestisce le credenziali dell'utente che si connette a un computer remoto utilizzando il Client Desktop remoto, che è stata introdotta in Windows 8. Le credenziali in formato non crittografato vengono inviate all'host di destinazione in cui l'host tenta di eseguire il processo di autenticazione e, se ha esito positivo, l'utente si connette alle risorse consentite. RDP non archivia le credenziali nel client, ma le credenziali di dominio dell'utente vengono archiviate in LSASS.

Introdotto in Windows Server 2012 R2 e Windows 8.1, modalità di amministrazione limitata offre sicurezza aggiuntiva per scenari di accesso remoto. In questa modalità di Desktop remoto, l'applicazione client per eseguire con la funzione unidirezionale NT (NTOWF) Accesso rete challenge / response o utilizzare un ticket di servizio Kerberos per l'autenticazione all'host remoto. Dopo che l'amministratore viene autenticato, l'amministratore non dispone le credenziali dell'account corrispondente in LSASS poiché non sono stati forniti all'host remoto. Al contrario, l'amministratore ha le credenziali dell'account computer per la sessione. Le credenziali di amministratore non vengono fornite all'host remoto, in modo che vengano eseguite azioni come l'account computer. Risorse anche sono limitate all'account del computer e l'amministratore non può accedere alle risorse con il proprio account.

### <a name="automatic-restart-sign-on-credential-process"></a>Processo di riavvio automatico delle credenziali di accesso
Quando un utente accede su un dispositivo Windows 8.1, LSA Salva le credenziali dell'utente nella memoria crittografata che sono accessibili solo a LSASS.exe. Quando Windows Update avvia un riavvio automatico senza presenza dell'utente, queste credenziali vengono utilizzate per configurare l'accesso automatico per l'utente.

L'utente è connesso automaticamente tramite il meccanismo di accesso automatico al riavvio, e quindi il computer viene inoltre bloccato per proteggere la sessione dell'utente. Il blocco viene avviato tramite Winlogon mentre viene eseguita la gestione delle credenziali tramite LSA. Per accedere automaticamente e bloccando la sessione dell'utente nella console, applicazioni schermata di blocco dell'utente è riavviate e disponibili.

Per ulteriori informazioni su Winlogon, vedere [Winlogon automatica riavviare Sign-On & #40; Winlogon & #41;](winlogon-automatic-restart-sign-on-arso.md).

### <a name="stored-user-names-and-passwords-in-windows-vista-and-windows-xp"></a>Gestione nomi utente e password in Windows Vista e Windows XP
In Windows Server 2008, Windows Server 2003, Windows Vista e Windows XP, **Gestione nomi utente e password** nel Pannello di controllo semplifica la gestione e l'utilizzo di più insiemi di credenziali di accesso, inclusi i certificati x. 509 usati con smart card e le credenziali Windows Live (ora chiamate account Microsoft). The credentials - part of the user's profile - are stored until needed. Questa azione può aumentare la sicurezza una base per ogni risorsa, assicurandosi che se una password è compromesso, comprometta tutte sicurezza.

Dopo che un utente accede e tenta di accedere a risorse aggiuntive protetta da password, ad esempio una condivisione in un server, e se le credenziali di accesso predefinito dell'utente non sono sufficienti per accedere, **Gestione nomi utente e password** viene eseguita una query. Se sono state salvate le informazioni di accesso corrette credenziali alternative **Gestione nomi utente e password**, queste credenziali vengono utilizzate per ottenere l'accesso. In caso contrario, l'utente viene richiesto di specificare nuove credenziali, che possono quindi essere salvate per il riutilizzo, più avanti in una sessione di accesso oppure durante una sessione successiva.

Si applicano le restrizioni seguenti:

-   Se **Gestione nomi utente e password** contiene le credenziali non valide o non corrette per una risorsa specifica, l'accesso alla risorsa viene negata e **Gestione nomi utente e password** non viene visualizzata la finestra di dialogo.

-   **I nomi utente e password archiviati** archivia le credenziali solo per il protocollo Kerberos, NTLM, account Microsoft (in precedenza Windows Live ID) e l'autenticazione Secure Sockets Layer (SSL). Alcune versioni di Internet Explorer consente di gestire i propri cache per l'autenticazione di base.

Queste credenziali diventano parte del profilo dell'utente locale nella directory Data\Microsoft\Credentials Settings\NomeUtente\Application \Documents and crittografata. Di conseguenza, queste credenziali possono effettuare il roaming con l'utente se criteri di rete dell'utente supporta i profili utente mobili. Tuttavia, se l'utente abbia copie del **Gestione nomi utente e password** su due computer diversi e modifiche le credenziali associate la risorsa in uno di questi computer, la modifica non viene distribuita a **Gestione nomi utente e password** nel secondo computer.

### <a name="windows-vault-and-credential-manager"></a>Insieme di credenziali Windows e gestione credenziali
Gestione credenziali è stato introdotto in Windows Server 2008 R2 e Windows 7 come una funzionalità del Pannello di controllo per archiviare e gestire i nomi utente e password. Gestione credenziali consente agli utenti di archiviare le credenziali relative ad altri sistemi e i siti Web nell'insieme di credenziali sicuro di Windows. Alcune versioni di Internet Explorer utilizzano questa funzionalità per l'autenticazione per siti Web.

Gestione delle credenziali mediante Gestione credenziali è controllata dall'utente nel computer locale. Gli utenti possono salvare e archiviare le credenziali dai browser supportati e le applicazioni di Windows per rendere più pratico quando hanno bisogno di accedere a queste risorse. Le credenziali vengono salvate in speciali cartelle crittografate nel computer sotto il profilo dell'utente. Le applicazioni che supportano questa funzionalità (utilizzando l'API di gestione credenziali), ad esempio i web browser e le app, possono presentare le credenziali corrette ad altri computer e i siti Web durante il processo di accesso.

Quando un sito Web, un'applicazione o un altro computer richieste l'autenticazione tramite il protocollo Kerberos o NTLM, una finestra di dialogo viene visualizzata in cui selezionare il **Aggiorna credenziali predefinite** o **salvare Password** casella di controllo. Questa finestra di dialogo che consente agli utenti di salvare le credenziali in locale viene generata da un'applicazione che supporta le API di gestione credenziali. Se l'utente seleziona il **salvare Password** casella di controllo, Gestione credenziali tiene traccia del nome utente dell'utente, password, e informazioni per il servizio di autenticazione in uso.

La volta successiva che il servizio viene utilizzato, Gestione credenziali fornisce automaticamente le credenziali archiviate nell'insieme di credenziali di Windows. Se queste non vengono accettate, l'utente è richiesto per le informazioni di accesso corrette. Se viene concesso l'accesso con le nuove credenziali, Gestione credenziali ultime sovrascrivono le precedenti con quello nuovo e quindi Archivia le nuove credenziali nell'insieme di credenziali di Windows.

## <a name="BKMK_SAM"></a>Database di gestione degli account di sicurezza
La gestione account di sicurezza (SAM) è un database che archivia i gruppi e account utente locale. È presente in ogni sistema operativo Windows. Tuttavia, quando un computer viene aggiunto a un dominio, Active Directory consente di gestire gli account di dominio in domini di Active Directory.

Ad esempio, computer client che eseguono un sistema operativo Windows partecipare a un dominio di rete mediante la comunicazione con un controller di dominio, anche quando nessun utente umano è connesso. Per avviare le comunicazioni, il computer deve disporre di un account attivo nel dominio. Prima di accettare le comunicazioni dal computer, LSA sul controller di dominio autentica l'identità del computer e quindi costruisce il contesto di sicurezza del computer così come avviene per le entità di sicurezza umano. Questo contesto di sicurezza definisce le identità e capacità di un utente o un servizio in un determinato computer o un utente, servizio o computer in una rete. For example, the access token contained within the security context defines the resources (such as a file share or printer) that can be accessed and the actions (such as Read, Write, or Modify) that can be performed by that principal - a user, computer, or service on that resource.

Il contesto di sicurezza di un utente o computer può variare da un computer a altro, ad esempio quando un utente accede a un workstation o un server diverso da workstation primario dell'utente. Possono anche variare da una sessione a altra, ad esempio quando un amministratore modifica diritti e autorizzazioni dell'utente. Inoltre, il contesto di sicurezza è in genere diverso quando un utente o computer funziona su base autonoma in una rete o come parte di un dominio Active Directory.

## <a name="BKMK_LocalDomainsAndTrustedDomains"></a>I domini locali e dei domini trusted
Quando esiste un trust tra due domini, i meccanismi di autenticazione per ogni dominio si basano sulla validità delle autenticazioni provenienti da altro dominio. Considera attendibile la Guida per fornire accesso controllato a risorse condivise in un dominio di risorse (il dominio trusting) verificando che le richieste di autenticazione in ingresso provenienti da un'autorità attendibile (il dominio trusted). In questo modo, i trust agiscono come bridge che consentono solo convalidata viaggio le richieste di autenticazione tra domini.

Come una relazione di trust specifico passa le richieste di autenticazione varia a seconda della configurazione. Relazioni di trust possono essere bidirezionale o unidirezionale, fornendo accesso dal dominio trusted alle risorse nel dominio trusting, fornendo accesso di ciascun dominio alle risorse in altro dominio. Relazioni di trust sono anche non transitivo, nel qual caso esiste una relazione di trust solo tra i domini di trust due tra partner o transitive, nel qual caso una relazione di trust estende automaticamente a eventuali altri domini trusting di entrambi i partner.

Per informazioni sulle relazioni di trust tra domini e foreste riguardanti l'autenticazione, vedere [dell'autenticazione delegata e relazioni di Trust](https://technet.microsoft.com/library/dn169022.aspx).

## <a name="BKMK_CertificatesInWindowsAuthentication"></a>Certificati di autenticazione di Windows
Un'infrastruttura a chiave pubblica (PKI) è la combinazione di software, le tecnologie di crittografia, processi e servizi che consentono di un'organizzazione proteggere la comunicazione e transazioni commerciali. La possibilità di un'infrastruttura PKI per proteggere le comunicazioni e transazioni commerciali dipende lo scambio di certificati digitali tra gli utenti autenticati e risorse attendibili.

Un certificato digitale è un documento elettronico che contiene informazioni relative all'entità che a cui appartiene, l'entità da che sia stato emesso, un numero di serie o alcuni altri identificazione univoca, rilascio e date di scadenza e un'impronta digitale.

L'autenticazione è il processo di determinare se un host remoto può essere considerato attendibile. Per stabilire l'attendibilità, l'host remoto deve fornire un certificato di autenticazione accettabile.

Gli host remoti stabilire i livelli di attendibilità per ottenere un certificato da un'autorità di certificazione (CA). L'autorità di certificazione può, a sua volta, avere certificazione da un'autorità superiore, che crea una catena di certificati. Per determinare se un certificato è attendibile, un'applicazione deve determinare l'identità della CA radice e quindi determinare se è attendibile.

Analogamente, il computer locale o un host remoto deve determinare se il certificato presentato dal utente o dell'applicazione è autentico. Il certificato presentato dall'utente tramite il LSA e SSPI viene valutato l'autenticità del computer locale per l'accesso locale, in rete o nel dominio tramite gli archivi certificati in Active Directory.

Per generare un certificato, i dati di autenticazione passa attraverso gli algoritmi hash, ad esempio Secure Hash Algorithm 1 (SHA1), per produrre un digest del messaggio. Il digest del messaggio viene quindi firmati digitalmente con chiave privata del mittente per dimostrare che il digest del messaggio è stato generato dal mittente.

> [!NOTE]
> SHA1 è l'impostazione predefinita in Windows 7 e Windows Vista, ma è stata modificata per SHA2 in Windows 8.

**Autenticazione con smart card**

Tecnologia delle smart card è un esempio di autenticazione basata su certificati. L'accesso a una rete con una smart card rappresenta un tipo di autenticazione sicuro perché Usa identificazione basati sulla crittografia e prova del possesso per l'autenticazione di un utente a un dominio. Servizi certificati Active Directory (AD CS) fornisce l'identificazione di crittografia basata su tramite il rilascio di un certificato di accesso per ogni smart card.

Per informazioni sull'autenticazione con smart card, vedere il [documentazione tecnica su Windows Smart Card](https://technet.microsoft.com/library/ff404297.aspx).

La tecnologia smart card virtuale è stata introdotta in Windows 8. Archivia il certificato della smart card nel PC e quindi si protegge con chip di sicurezza del dispositivo a prova di manomissione modulo TPM (Trusted Platform). In questo modo, il PC diventa effettivamente la smart card che devono ricevere il PIN dell'utente per l'autenticazione.

**Autenticazione remota e wireless**

Autenticazione di rete wireless e remoto è un'altra tecnologia che utilizza certificati per l'autenticazione. Il servizio di autenticazione Internet (IAS) e i server di rete privata virtuale utilizzare Extensible Authentication Protocol-Transport Level Security (EAP-TLS), PEAP Protected Extensible Authentication Protocol () o Internet Protocol security (IPsec) per eseguire l'autenticazione basata su certificati per molti tipi di accesso alla rete, tra cui la rete privata virtuale (VPN) e le connessioni wireless.

For information about certificate-based authentication in networking, see [Network access authentication and certificates](https://technet.microsoft.com/library/cc759575(WS.10).aspx).

## <a name="BKMK_SeeAlso"></a>Vedere anche
[Concetti di autenticazione di Windows](https://technet.microsoft.com/library/d169018.aspx)


