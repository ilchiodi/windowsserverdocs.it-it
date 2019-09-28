---
title: Processi di credenziali in Autenticazione di Windows
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 051cb88620065ed675f377f3369860f7b04460bd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402282"
---
# <a name="credentials-processes-in-windows-authentication"></a>Processi di credenziali in Autenticazione di Windows

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento di riferimento per i professionisti IT descrive il modo in cui l'autenticazione di Windows elabora le credenziali.

Gestione delle credenziali di Windows è il processo mediante il quale il sistema operativo riceve le credenziali dal servizio o dall'utente e protegge tali informazioni per una presentazione futura alla destinazione di autenticazione. Nel caso di un computer aggiunto al dominio, la destinazione di autenticazione è il controller di dominio. Le credenziali utilizzate per l'autenticazione sono documenti digitali che associano l'identità dell'utente a una forma di verifica dell'autenticità, ad esempio un certificato, una password o un PIN.

Per impostazione predefinita, le credenziali di Windows vengono convalidate in base al database di gestione degli account di sicurezza (SAM) nel computer locale o a Active Directory in un computer appartenente a un dominio tramite il servizio Winlogon. Le credenziali vengono raccolte tramite l'input dell'utente nell'interfaccia utente di accesso o a livello di codice tramite il Application Programming Interface (API) da presentare alla destinazione di autenticazione.

Le informazioni di sicurezza locali vengono archiviate nel registro di sistema in **HKEY_LOCAL_MACHINE\SECURITY**. Le informazioni archiviate includono le impostazioni dei criteri, i valori di sicurezza predefiniti e le informazioni sull'account, ad esempio le credenziali di accesso nella cache. Una copia del database SAM viene anche archiviata qui, sebbene sia protetta da scrittura.

Il diagramma seguente illustra i componenti necessari e i percorsi che le credenziali prendono attraverso il sistema per autenticare l'utente o il processo per un accesso riuscito.

![Diagramma che mostra i componenti necessari e i percorsi che le credenziali prendono attraverso il sistema per autenticare l'utente o il processo per un accesso riuscito.](../media/credentials-processes-in-windows-authentication/AuthN_LSA_Architecture_Client.gif)

Nella tabella seguente vengono descritti i componenti che gestiscono le credenziali nel processo di autenticazione al momento dell'accesso.

**Componenti di autenticazione per tutti i sistemi**

|Componente|Descrizione|
|-------|--------|
|Accesso dell'utente|Winlogon. exe è il file eseguibile responsabile della gestione delle interazioni utente protette. Il servizio Winlogon avvia il processo di accesso per i sistemi operativi Windows passando le credenziali raccolte dall'azione dell'utente sul desktop sicuro (interfaccia utente di accesso) all'autorità di sicurezza locale (LSA) tramite Secur32. dll.|
|Accesso all'applicazione|Accessi dell'applicazione o del servizio che non richiedono l'accesso interattivo. La maggior parte dei processi avviati dall'utente viene eseguita in modalità utente utilizzando Secur32. dll, mentre i processi avviati all'avvio, ad esempio i servizi, vengono eseguiti in modalità kernel utilizzando Ksecdd. sys.<br /><br />Per ulteriori informazioni sulla modalità utente e sulla modalità kernel, vedere applicazioni e modalità utente o servizi e modalità kernel in questo argomento.|
|Secur32.dll|I provider di autenticazione multipli che costituiscono la base del processo di autenticazione.|
|Lsasrv.dll|Il servizio del server LSA, che applica entrambi i criteri di sicurezza e funge da Gestione pacchetti di sicurezza per LSA. L'LSA contiene la funzione Negotiate, che seleziona il protocollo NTLM o Kerberos dopo aver determinato il protocollo che deve avere esito positivo.|
|Security Support Provider|Set di provider che possono richiamare singolarmente uno o più protocolli di autenticazione. Il set predefinito di provider può essere modificato con ogni versione del sistema operativo Windows e i provider personalizzati possono essere scritti.|
|Netlogon.dll|I servizi eseguiti dal servizio Accesso rete sono i seguenti:<br /><br />-Mantiene il canale sicuro del computer (da non confondere con Schannel) in un controller di dominio.<br />: Passa le credenziali dell'utente tramite un canale protetto al controller di dominio e restituisce gli identificatori di sicurezza (SID) del dominio e i diritti utente per l'utente.<br />: Pubblica i record di risorse del servizio nel Domain Name System (DNS) e usa DNS per risolvere i nomi negli indirizzi IP (Internet Protocol) dei controller di dominio.<br />: Implementa il protocollo di replica basato su RPC (Remote Procedure Call) per la sincronizzazione dei controller di dominio primario (PDC) e dei controller di dominio di backup (I BDC).|
|Samsrv. dll|Gestione account di sicurezza (SAM), che archivia gli account di sicurezza locali, applica i criteri archiviati localmente e supporta le API.|
|Registro di sistema|Il registro di sistema contiene una copia del database SAM, le impostazioni dei criteri di sicurezza locali, i valori di sicurezza predefiniti e le informazioni sull'account accessibili solo al sistema.|

Questo argomento è suddiviso nelle sezioni seguenti:

-   [Input delle credenziali per l'accesso utente](#BKMK_CrentialInputForUserLogon)

-   [Input delle credenziali per l'accesso all'applicazione e al servizio](#BKMK_CredentialInputForApplicationAndServiceLogon)

-   [Autorità di sicurezza locale](#BKMK_LSA)

-   [Credenziali e convalida memorizzate nella cache](#BKMK_CachedCredentialsAndValidation)

-   [Archiviazione e convalida delle credenziali](#BKMK_CredentialStorageAndValidation)

-   [Database di gestione degli account di sicurezza](#BKMK_SAM)

-   [Domini locali e domini trusted](#BKMK_LocalDomainsAndTrustedDomains)

-   [Certificati nell'autenticazione di Windows](#BKMK_CertificatesInWindowsAuthentication)

## <a name="BKMK_CrentialInputForUserLogon"></a>Input delle credenziali per l'accesso utente
In Windows Server 2008 e Windows Vista, l'architettura GINA (Graphical Identification and Authentication) è stata sostituita da un modello di provider di credenziali, che consente di enumerare diversi tipi di accesso tramite l'utilizzo dei riquadri di accesso. Entrambi i modelli sono descritti di seguito.

**Architettura di identificazione e autenticazione grafica**

L'architettura GINA (Graphical Identification and Authentication) si applica ai sistemi operativi Windows Server 2003, Microsoft Windows 2000 Server, Windows XP e Windows 2000 Professional. In questi sistemi, ogni sessione di accesso interattivo crea un'istanza separata del servizio Winlogon. L'architettura GINA viene caricata nello spazio di processo usato da Winlogon, riceve ed elabora le credenziali ed esegue le chiamate alle interfacce di autenticazione tramite LSALogonUser.

Le istanze di Winlogon per un accesso interattivo vengono eseguite nella sessione 0. La sessione 0 ospita i servizi di sistema e altri processi critici, incluso il processo dell'autorità di sicurezza locale (LSA).

Il diagramma seguente illustra il processo di credenziale per Windows Server 2003, Microsoft Windows 2000 Server, Windows XP e Microsoft Windows 2000 Professional.

![Diagramma che illustra il processo di credenziale per Windows Server 2003, Microsoft Windows 2000 Server, Windows XP e Microsoft Windows 2000 Professional](../media/credentials-processes-in-windows-authentication/AuthN_GINA_Architecture.gif)

**Architettura del provider di credenziali**

L'architettura del provider di credenziali si applica alle versioni indicate nell'elenco **si applica a** all'inizio di questo argomento. In questi sistemi, l'architettura di input delle credenziali è stata modificata in una progettazione estendibile usando i provider di credenziali. Questi provider sono rappresentati da diversi riquadri di accesso sul desktop protetto che consentono un numero qualsiasi di scenari di accesso, ovvero account diversi per lo stesso utente e metodi di autenticazione diversi, ad esempio password, smart card e biometria.

Con l'architettura del provider di credenziali, Winlogon avvia sempre l'interfaccia utente di accesso dopo aver ricevuto un evento di sequenza di attenzione sicura. L'interfaccia utente di accesso esegue una query su ogni provider di credenziali per il numero di tipi di credenziali diversi configurati dal provider per l'enumerazione. I provider di credenziali hanno la possibilità di specificare uno di questi riquadri come valore predefinito. Dopo che tutti i provider hanno enumerato i riquadri, l'interfaccia utente di accesso li visualizza all'utente. L'utente interagisce con un riquadro per fornire le proprie credenziali. L'interfaccia utente di accesso Invia le credenziali per l'autenticazione.

I provider di credenziali non sono meccanismi di imposizione. Vengono usati per raccogliere e serializzare le credenziali. L'autorità di sicurezza locale e i pacchetti di autenticazione applicano la sicurezza.

I provider di credenziali sono registrati nel computer e sono responsabili dei seguenti elementi:

-   Descrizione delle informazioni sulle credenziali necessarie per l'autenticazione.

-   Gestione della comunicazione e della logica con autorità di autenticazione esterne.

-   Creazione di pacchetti di credenziali per l'accesso interattivo e di rete.

La creazione di pacchetti di credenziali per l'accesso interattivo e di rete include il processo di serializzazione. Serializzando le credenziali è possibile visualizzare più riquadri di accesso nell'interfaccia utente di accesso. L'organizzazione può quindi controllare la visualizzazione di accesso, ad esempio gli utenti, i sistemi di destinazione per l'accesso, l'accesso con pre-accesso alla rete e i criteri di blocco/sblocco della workstation, tramite l'uso di provider di credenziali personalizzati. Più provider di credenziali possono coesistere nello stesso computer.

I provider Single Sign-on (SSO) possono essere sviluppati come provider di credenziali standard o come provider di accesso di pre-accesso.

Ogni versione di Windows contiene un provider di credenziali predefinito e un provider di accesso pre-accesso predefinito (PLAP), noto anche come provider SSO. Il provider SSO consente agli utenti di effettuare una connessione a una rete prima di accedere al computer locale. Quando questo provider viene implementato, il provider non enumera i riquadri nell'interfaccia utente di accesso.

Un provider SSO può essere utilizzato negli scenari seguenti:

-   **L'autenticazione di rete e l'accesso al computer vengono gestiti da diversi provider di credenziali.** Le varianti a questo scenario includono:

    -   Un utente ha la possibilità di connettersi a una rete, ad esempio la connessione a una rete privata virtuale (VPN), prima di accedere al computer, ma non è necessario per effettuare questa connessione.

    -   L'autenticazione di rete è necessaria per recuperare le informazioni utilizzate durante l'autenticazione interattiva nel computer locale.

    -   Più autenticazioni di rete sono seguite da uno degli altri scenari. Ad esempio, un utente esegue l'autenticazione a un provider di servizi Internet (ISP), esegue l'autenticazione a una VPN e quindi usa le credenziali dell'account utente per accedere localmente.

    -   Le credenziali memorizzate nella cache sono disabilitate e una connessione dei servizi di accesso remoto tramite VPN è richiesta prima dell'accesso locale per autenticare l'utente.

    -   Un utente di dominio non dispone di un account locale configurato in un computer aggiunto a un dominio e deve stabilire una connessione a servizi di accesso remoto tramite la connessione VPN prima di completare l'accesso interattivo.

-   **L'autenticazione di rete e l'accesso al computer vengono gestiti dallo stesso provider di credenziali.** In questo scenario, è necessario che l'utente si connetta alla rete prima di accedere al computer.

**Enumerazione riquadro di accesso**

Il provider di credenziali enumera i riquadri di accesso nelle istanze seguenti:

-   Per i sistemi operativi designati nell'elenco **si applica a** all'inizio di questo argomento.

-   Il provider di credenziali enumera i riquadri per l'accesso alla workstation. Il provider di credenziali serializza in genere le credenziali per l'autenticazione nell'autorità di sicurezza locale. Questo processo Visualizza i riquadri specifici per ogni utente e specifici per i sistemi di destinazione di ogni utente.

-   L'architettura di accesso e autenticazione consente a un utente di utilizzare i riquadri enumerati dal provider di credenziali per sbloccare una workstation. In genere, l'utente attualmente connesso è il riquadro predefinito, ma se più di un utente è connesso, vengono visualizzati numerosi riquadri.

-   Il provider di credenziali enumera i riquadri in risposta a una richiesta dell'utente per modificare la password o altre informazioni private, ad esempio un PIN. In genere, l'utente attualmente connesso è il riquadro predefinito; Tuttavia, se più di un utente è connesso, vengono visualizzati numerosi riquadri.

-   Il provider di credenziali enumera i riquadri in base alle credenziali serializzate da utilizzare per l'autenticazione nei computer remoti. Nell'interfaccia utente delle credenziali non viene utilizzata la stessa istanza del provider dell'interfaccia utente di accesso, di sblocco della workstation o di modifica della password. Non è pertanto possibile mantenere le informazioni sullo stato nel provider tra le istanze dell'interfaccia utente delle credenziali. Questa struttura restituisce un riquadro per ogni accesso al computer remoto, supponendo che le credenziali siano state serializzate correttamente. Questo scenario viene usato anche in controllo dell'account utente, che consente di impedire modifiche non autorizzate a un computer richiedendo all'utente l'autorizzazione o una password di amministratore prima di consentire azioni che potrebbero influire sull'operazione del computer in alternativa, è possibile modificare le impostazioni che interessano altri utenti del computer.

Il diagramma seguente illustra il processo di credenziale per i sistemi operativi designati nell'elenco **si applica a** all'inizio di questo argomento.

![Diagramma che illustra il processo di credenziale per i sistemi operativi indicati nell'elenco * * si applica a * * all'inizio di questo argomento](../media/credentials-processes-in-windows-authentication/AuthN_CredMan_CredProv.gif)

## <a name="BKMK_CredentialInputForApplicationAndServiceLogon"></a>Input delle credenziali per l'accesso all'applicazione e al servizio
L'autenticazione di Windows è progettata per gestire le credenziali per le applicazioni o i servizi che non richiedono l'interazione dell'utente. Le applicazioni in modalità utente sono limitate in termini di risorse di sistema a cui hanno accesso, mentre i servizi possono avere accesso illimitato alla memoria di sistema e ai dispositivi esterni.

I servizi di sistema e le applicazioni a livello di trasporto accedono a un Security Support Provider (SSP) tramite Security Support Provider Interface (SSPI) in Windows, che fornisce funzioni per l'enumerazione dei pacchetti di sicurezza disponibili in un sistema, la selezione di un e usare il pacchetto per ottenere una connessione autenticata.

Quando viene autenticata una connessione client/server:

-   L'applicazione sul lato client della connessione Invia le credenziali al server utilizzando la funzione SSPI `InitializeSecurityContext (General)`.

-   L'applicazione sul lato server della connessione risponde con la funzione SSPI `AcceptSecurityContext (General)`.

-   Le funzioni SSPI `InitializeSecurityContext (General)` e `AcceptSecurityContext (General)` vengono ripetute fino a quando tutti i messaggi di autenticazione necessari non vengono scambiati con l'autenticazione riuscita o non riuscita.

-   Dopo che la connessione è stata autenticata, l'LSA sul server utilizza le informazioni del client per compilare il contesto di sicurezza, che contiene un token di accesso.

-   Il server può quindi chiamare la funzione SSPI `ImpersonateSecurityContext` per alleghiare il token di accesso a un thread di rappresentazione per il servizio.

**Applicazioni e modalità utente**

La modalità utente in Windows è costituita da due sistemi in grado di passare le richieste di I/O ai driver in modalità kernel appropriati: il sistema dell'ambiente, che esegue applicazioni scritte per molti tipi diversi di sistemi operativi, e il sistema integrale, che funziona funzioni specifiche del sistema per conto del sistema dell'ambiente.

Il sistema integrale gestisce le funzioni system'specific operative per conto del sistema dell'ambiente ed è costituito da un processo di sistema di sicurezza (LSA), da un servizio workstation e da un servizio Server. Il processo del sistema di sicurezza gestisce i token di sicurezza, concede o nega le autorizzazioni per accedere agli account utente in base alle autorizzazioni delle risorse, gestisce le richieste di accesso e avvia l'autenticazione di accesso e determina le risorse di sistema del sistema operativo deve eseguire il controllo.

Le applicazioni possono essere eseguite in modalità utente in cui l'applicazione può essere eseguita come qualsiasi entità, incluso nel contesto di sicurezza del sistema locale (sistema). Le applicazioni possono anche essere eseguite in modalità kernel in cui l'applicazione può essere eseguita nel contesto di sicurezza del sistema locale (sistema).

SSPI è disponibile tramite il modulo secur32. dll, un'API utilizzata per ottenere servizi di sicurezza integrati per l'autenticazione, l'integrità dei messaggi e la privacy dei messaggi. Fornisce un livello di astrazione tra i protocolli a livello di applicazione e i protocolli di sicurezza. Poiché le diverse applicazioni richiedono diversi modi per identificare o autenticare gli utenti e modi diversi di crittografare i dati durante il trasferimento in rete, SSPI fornisce un modo per accedere alle librerie di collegamento dinamico (dll) che contengono un'autenticazione diversa e funzioni crittografiche. Queste dll sono denominate SSP (Security Support Provider).

Gli account dei servizi gestiti e gli account virtuali sono stati introdotti in Windows Server 2008 R2 e Windows 7 per fornire applicazioni cruciali, ad esempio Microsoft SQL Server e Internet Information Services (IIS), con l'isolamento dei propri account di dominio, mentre eliminando la necessità di un amministratore di amministrare manualmente il nome dell'entità servizio (SPN) e le credenziali per questi account. Per ulteriori informazioni su queste funzionalità e sul relativo ruolo nell'autenticazione, vedere la [documentazione relativa agli account dei servizi gestiti per Windows 7 e Windows Server 2008 R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx) e [Panoramica degli account del servizio gestito del gruppo](../group-managed-service-accounts/group-managed-service-accounts-overview.md).

**Servizi e modalità kernel**

Anche se la maggior parte delle applicazioni Windows viene eseguita nel contesto di sicurezza dell'utente che li avvia, questo non è vero per i servizi. Molti servizi di Windows, ad esempio servizi di stampa e di rete, vengono avviati dal controller del servizio quando l'utente avvia il computer. Questi servizi possono essere eseguiti come servizio locale o sistema locale e potrebbero continuare a essere eseguiti dopo che l'ultimo utente si disconnette.

> [!NOTE]
> I servizi vengono in genere eseguiti in contesti di sicurezza noti come sistema locale (sistema), servizio di rete o servizio locale.  Windows Server 2008 R2 ha introdotto servizi che vengono eseguiti con un account del servizio gestito, che sono entità di dominio.

Prima di avviare un servizio, il controller di servizio esegue l'accesso utilizzando l'account designato per il servizio, quindi presenta le credenziali del servizio per l'autenticazione da parte di LSA. Il servizio Windows implementa un'interfaccia a livello di codice che può essere utilizzata da Gestione controller di servizi per controllare il servizio. Un servizio Windows può essere avviato automaticamente all'avvio del sistema o manualmente con un programma di controllo del servizio. Ad esempio, quando un computer client Windows viene aggiunto a un dominio, il servizio Messenger nel computer si connette a un controller di dominio e apre un canale sicuro. Per ottenere una connessione autenticata, il servizio deve disporre di credenziali attendibili per l'autorità di protezione locale (LSA) del computer remoto. Quando si comunica con altri computer della rete, LSA usa le credenziali per l'account di dominio del computer locale, così come tutti gli altri servizi in esecuzione nel contesto di sicurezza del sistema locale e del servizio di rete. I servizi nel computer locale vengono eseguiti come sistema, pertanto non è necessario che le credenziali vengano presentate all'LSA.

Il file Ksecdd. sys gestisce e crittografa le credenziali e utilizza una chiamata di procedura locale nell'LSA. Il tipo di file è DRV (driver) ed è noto come SSP (Security Support Provider) in modalità kernel e, nelle versioni indicate nell'elenco **si applica a** all'inizio di questo argomento, è conforme a FIPS 140-2 Level 1.

La modalità kernel ha accesso completo alle risorse hardware e di sistema del computer. La modalità kernel interrompe l'accesso alle aree critiche del sistema operativo da parte dei servizi e delle applicazioni in modalità utente a cui non dovrebbero accedere.

## <a name="BKMK_LSA"></a>Autorità di sicurezza locale
L'autorità di sicurezza locale (LSA) è un processo di sistema protetto che consente di autenticare e registrare gli utenti nel computer locale. Inoltre, LSA mantiene le informazioni su tutti gli aspetti della sicurezza locale in un computer (questi aspetti sono collettivamente noti come criteri di sicurezza locali) e fornisce vari servizi per la conversione tra i nomi e gli identificatori di sicurezza (SID). Il processo del sistema di sicurezza, LSASS (Local Security Authority Server Service), tiene traccia dei criteri di sicurezza e degli account attivi in un sistema di computer.

LSA convalida l'identità di un utente in base a quale delle due entità seguenti ha emesso l'account dell'utente:

-   **Autorità di sicurezza locale.** L'LSA può convalidare le informazioni utente controllando il database di gestione degli account di sicurezza (SAM) presente nello stesso computer. Qualsiasi workstation o server membro può archiviare account utente locali e informazioni sui gruppi locali. Tuttavia, questi account possono essere usati per accedere solo a tale workstation o computer.

-   **Autorità di sicurezza per il dominio locale o per un dominio trusted.** Il LSA Contatta l'entità che ha emesso l'account e richiede la verifica che l'account sia valido e che la richiesta sia stata originata dal titolare dell'account.

Il servizio LSASS (Local Security Authority Subsystem Service) permette di archiviare credenziali in memoria per conto di utenti con sessioni attive di Windows. Le credenziali archiviate consentono agli utenti di accedere facilmente alle risorse di rete, ad esempio condivisioni file, cassette postali di Exchange Server e siti di SharePoint, senza immettere di nuovo le credenziali per ogni servizio remoto.

Il servizio LSASS permette di archiviare credenziali in più formati, inclusi i seguenti:

-   Testo normale crittografato in modo reversibile

-   Ticket Kerberos (ticket-concessione ticket (TGT), ticket di servizio)

-   Hash NT

-   Hash di LAN Manager (LM)

Se l'utente accede a Windows usando una smart card, LSASS non archivia una password in testo non crittografato, ma archivia il valore hash NT corrispondente per l'account e il PIN in testo non crittografato per la smart card. Se l'attributo dell'account è abilitato per una smart card necessaria per l'accesso interattivo, un valore di hash NT casuale viene generato automaticamente per l'account invece dell'hash della password originale. L'hash di password generato automaticamente quando si imposta l'attributo non viene modificato.

Se un utente accede a un computer basato su Windows con una password compatibile con gli hash di LAN Manager (LM), questo autenticatore è presente in memoria.

L'archiviazione di credenziali in testo non crittografato in memoria non può essere disabilitata, anche se i provider di credenziali che le richiedono sono disabilitati.

Le credenziali archiviate sono associate direttamente alle sessioni di accesso Local Security Authority Subsystem Service (LSASS) che sono state avviate dopo l'ultimo riavvio e non sono state chiuse. Ad esempio, sessioni LSA con credenziali LSA archiviate vengono create quando un utente esegue una delle operazioni seguenti:

-   Accede a una sessione locale o a una sessione di Remote Desktop Protocol (RDP) nel computer

-   Esecuzione di un task con l'opzione **RunAs**

-   Esecuzione di un servizio di Windows attivo nel computer

-   Esecuzione di un'attività o un processo batch pianificato

-   Esecuzione di un'attività nel computer locale tramite uno strumento di amministrazione remota

In alcune circostanze, i segreti LSA, che sono parti segrete dei dati accessibili solo ai processi di account di sistema, vengono archiviati nell'unità disco rigido. Alcuni di questi segreti sono credenziali che devono essere mantenute dopo un riavvio e sono archiviate in formato crittografato nell'unità disco rigido. Le credenziali archiviate come segreti LSA possono includere:

-   Password dell'account per l'account Active Directory Domain Services (AD DS) del computer

-   Password di account per servizi di Windows configurati nel computer

-   Password di account per le attività pianificate configurate

-   Password di account per pool di applicazioni IIS e siti Web

-   Password per gli account Microsoft

Introdotta in Windows 8.1, il sistema operativo client fornisce protezione aggiuntiva per LSA per impedire la lettura di memoria e l'inserimento di codice da processi non protetti. Questa protezione consente di aumentare la sicurezza per le credenziali archiviate e gestite da LSA.

Per ulteriori informazioni su queste protezioni aggiuntive, vedere la pagina relativa alla [configurazione della protezione LSA aggiuntiva](../credentials-protection-and-management/configuring-additional-lsa-protection.md).

## <a name="BKMK_CachedCredentialsAndValidation"></a>Credenziali e convalida memorizzate nella cache
I meccanismi di convalida si basano sulla presentazione di credenziali al momento dell'accesso. Tuttavia, quando il computer è disconnesso da un controller di dominio e l'utente presenta le credenziali del dominio, Windows usa il processo di credenziali memorizzate nella cache nel meccanismo di convalida.

Ogni volta che un utente accede a un dominio, Windows memorizza nella cache le credenziali fornite e le archivia nell'hive di sicurezza nel registro di sistema del sistema operativo.

Con le credenziali memorizzate nella cache, l'utente può accedere a un membro del dominio senza connettersi a un controller di dominio all'interno di tale dominio.


## <a name="BKMK_CredentialStorageAndValidation"></a>Archiviazione e convalida delle credenziali
Non è sempre consigliabile usare un set di credenziali per l'accesso a risorse diverse. È ad esempio possibile che un amministratore desideri utilizzare le credenziali amministrative anziché le credenziali utente per l'accesso a un server remoto. Analogamente, se un utente accede a risorse esterne, ad esempio un conto bancario, può utilizzare solo credenziali diverse dalle credenziali di dominio. Nelle sezioni seguenti vengono descritte le differenze di gestione delle credenziali tra le versioni correnti dei sistemi operativi Windows e i sistemi operativi Windows Vista e Windows XP.

### <a name="remote-logon-credential-processes"></a>Processi di credenziali di accesso remoto
Il Remote Desktop Protocol (RDP) gestisce le credenziali dell'utente che si connette a un computer remoto utilizzando il client di Desktop remoto, introdotto in Windows 8. Le credenziali in formato testo non crittografato vengono inviate all'host di destinazione in cui l'host tenta di eseguire il processo di autenticazione e, in caso di esito positivo, connette l'utente alle risorse consentite. RDP non archivia le credenziali nel client, ma le credenziali di dominio dell'utente vengono archiviate in LSASS.

Introdotta in Windows Server 2012 R2 e Windows 8.1, la modalità di amministrazione limitata offre protezione aggiuntiva per gli scenari di accesso remoto. Questa modalità di Desktop remoto fa in modo che l'applicazione client esegua una richiesta di accesso alla rete con la funzione unidirezionale NT (NTOWF) o utilizzi un ticket di servizio Kerberos per l'autenticazione all'host remoto. Dopo l'autenticazione dell'amministratore, l'amministratore non ha le rispettive credenziali dell'account in LSASS perché non sono state fornite all'host remoto. L'amministratore dispone invece delle credenziali dell'account computer per la sessione. Le credenziali di amministratore non vengono fornite all'host remoto, quindi le azioni vengono eseguite come account computer. Le risorse sono limitate anche all'account del computer e l'amministratore non è in grado di accedere alle risorse con il proprio account.

### <a name="automatic-restart-sign-on-credential-process"></a>Processo di credenziali di accesso automatico per il riavvio
Quando un utente accede a un dispositivo Windows 8.1, LSA Salva le credenziali utente nella memoria crittografata accessibile solo da LSASS. exe. Quando Windows Update avvia un riavvio automatico senza presenza dell'utente, queste credenziali vengono usate per configurare l'accesso automatico per l'utente.

Al riavvio, l'utente viene automaticamente connesso tramite il meccanismo autologo, quindi il computer viene bloccato per proteggere la sessione dell'utente. Il blocco viene avviato tramite Winlogon, mentre la gestione delle credenziali viene eseguita da LSA. Eseguendo automaticamente l'accesso e il blocco della sessione dell'utente nella console, le applicazioni della schermata di blocco dell'utente vengono riavviate e disponibili.

Per ulteriori informazioni su al riavvio Winlogon, vedere la pagina relativa al [riavvio automatico &#40;di&#41;Winlogon al riavvio Winlogon](winlogon-automatic-restart-sign-on-arso.md).

### <a name="stored-user-names-and-passwords-in-windows-vista-and-windows-xp"></a>Nomi utente e password archiviati in Windows Vista e Windows XP
In Windows Server 2008, Windows Server 2003, Windows Vista e Windows XP, **i nomi utente e le password archiviati** nel pannello di controllo semplificano la gestione e l'utilizzo di più set di credenziali di accesso, inclusi i certificati X. 509 utilizzati con le smart card e Windows Live Credentials (ora denominato account Microsoft). Le credenziali, parte del profilo dell'utente, vengono archiviate fino a quando non sono necessarie. Questa azione consente di aumentare la sicurezza in base alle singole risorse garantendo che se una password viene compromessa, non compromette tutta la sicurezza.

Dopo che un utente si è connesso e tenta di accedere ad altre risorse protette da password, ad esempio una condivisione in un server, e se le credenziali di accesso predefinite dell'utente non sono sufficienti per ottenere l'accesso, vengono eseguite query su **nomi utente e password archiviati** . Se le credenziali alternative con le informazioni di accesso corrette sono state salvate in **nomi utente e password archiviati**, queste credenziali vengono utilizzate per ottenere l'accesso. In caso contrario, all'utente viene richiesto di fornire nuove credenziali, che possono quindi essere salvate per il riutilizzo, in un secondo momento nella sessione di accesso o durante una sessione successiva.

Si applicano le restrizioni seguenti:

-   Se i **nomi utente e le password archiviati** contengono credenziali non valide o non corrette per una risorsa specifica, l'accesso alla risorsa viene negato e la finestra di dialogo **nomi utente e password archiviati** non viene visualizzata.

-   **I nomi utente e le password archiviati** archiviano le credenziali solo per NTLM, protocollo Kerberos, account Microsoft (in precedenza Windows Live ID) e l'autenticazione Secure Sockets Layer (SSL). Alcune versioni di Internet Explorer gestiscono la propria cache per l'autenticazione di base.

Queste credenziali diventano una parte crittografata del profilo locale di un utente nella directory \Documents e Settings\nomeutente\dati Applicazioni\microsoft\credentials Di conseguenza, queste credenziali possono essere spostate con l'utente se i criteri di rete dell'utente supportano i profili utente mobili. Tuttavia, se l'utente dispone di copie di **nomi utente e password archiviati** in due computer diversi e modifica le credenziali associate alla risorsa in uno di questi computer, la modifica non viene propagata a **nomi utente e password archiviati.** nel secondo computer.

### <a name="windows-vault-and-credential-manager"></a>Windows Vault e gestione credenziali
Gestione credenziali è stato introdotto in Windows Server 2008 R2 e Windows 7 come funzionalità del pannello di controllo per archiviare e gestire i nomi utente e le password. Gestione credenziali consente agli utenti di archiviare le credenziali relative ad altri sistemi e siti Web nell'insieme di credenziali di Windows sicuro. Alcune versioni di Internet Explorer utilizzano questa funzionalità per l'autenticazione nei siti Web.

La gestione delle credenziali mediante Gestione credenziali è controllata dall'utente nel computer locale. Gli utenti possono salvare e archiviare le credenziali dai browser supportati e dalle applicazioni Windows in modo da rendere più pratico l'accesso a queste risorse. Le credenziali vengono salvate in cartelle speciali crittografate nel computer sotto il profilo dell'utente. Le applicazioni che supportano questa funzionalità (tramite l'utilizzo delle API di gestione delle credenziali), ad esempio Web browser e app, possono presentare le credenziali corrette ad altri computer e siti Web durante il processo di accesso.

Quando un sito Web, un'applicazione o un altro computer richiede l'autenticazione tramite NTLM o il protocollo Kerberos, viene visualizzata una finestra di dialogo in cui è possibile selezionare la casella di controllo **Aggiorna credenziali predefinite** o **Salva password** . Questa finestra di dialogo che consente a un utente di salvare le credenziali localmente viene generata da un'applicazione che supporta le API di gestione credenziali. Se l'utente seleziona la casella di controllo **Salva password** , gestione credenziali tiene traccia del nome utente, della password e delle informazioni correlate per il servizio di autenticazione in uso.

La volta successiva che viene utilizzato il servizio, gestione credenziali fornisce automaticamente le credenziali archiviate nell'insieme di credenziali di Windows. Se queste non vengono accettate, le informazioni di accesso corrette vengono richieste all'utente. Se l'accesso viene concesso con le nuove credenziali, gestione credenziali sovrascrive la credenziale precedente con quella nuova, quindi archivia le nuove credenziali nell'insieme di credenziali di Windows.

## <a name="BKMK_SAM"></a>Database di gestione degli account di sicurezza
Gestione account di protezione (SAM) è un database di in cui vengono archiviati gli account utente e i gruppi locali. È presente in ogni sistema operativo Windows; Tuttavia, quando un computer viene aggiunto a un dominio, Active Directory gestisce gli account di dominio nei domini Active Directory.

I computer client che eseguono un sistema operativo Windows, ad esempio, fanno parte di un dominio di rete comunicando con un controller di dominio anche quando nessun utente umano è connesso. Per avviare le comunicazioni, il computer deve disporre di un account attivo nel dominio. Prima di accettare le comunicazioni dal computer, LSA sul controller di dominio autentica l'identità del computer e quindi costruisce il contesto di sicurezza del computer esattamente come per un'entità di protezione umana. Questo contesto di sicurezza definisce l'identità e le funzionalità di un utente o di un servizio su un computer o un utente, un servizio o un computer specifico in una rete. Ad esempio, il token di accesso contenuto nel contesto di sicurezza definisce le risorse (ad esempio una condivisione file o una stampante) a cui è possibile accedere e le azioni, ad esempio lettura, scrittura o modifica, che possono essere eseguite da tale entità, ovvero un utente, un computer o un servizio su tale risorse.

Il contesto di sicurezza di un utente o di un computer può variare da un computer a un altro, ad esempio quando un utente accede a un server o a una workstation diversa dalla workstation primaria dell'utente. Può anche variare da una sessione a un'altra, ad esempio quando un amministratore modifica i diritti e le autorizzazioni dell'utente. Inoltre, il contesto di sicurezza è in genere diverso quando un utente o un computer opera su una base autonoma, in una rete o come parte di un dominio Active Directory.

## <a name="BKMK_LocalDomainsAndTrustedDomains"></a>Domini locali e domini trusted
Quando esiste una relazione di trust tra due domini, i meccanismi di autenticazione per ogni dominio si basano sulla validità delle autenticazioni provenienti dall'altro dominio. Consente di garantire l'accesso controllato alle risorse condivise in un dominio di risorse (dominio trusting) verificando che le richieste di autenticazione in ingresso provengano da un'autorità attendibile (dominio trusted). In questo modo, i trust fungono da Bridge che consentono solo la convalida delle richieste di autenticazione tra domini.

Il modo in cui un trust specifico passa le richieste di autenticazione dipende dalla configurazione. Le relazioni di trust possono essere unidirezionali, fornendo l'accesso dal dominio trusted alle risorse nel dominio trusting, oppure bidirezionale, fornendo l'accesso da ogni dominio alle risorse nell'altro dominio. I trust sono anche non transitivi, nel qual caso esiste una relazione di trust tra i due domini trust partner, o transitiva, nel qual caso un trust si estende automaticamente a qualsiasi altro dominio considerato attendibile da uno dei partner.

Per informazioni sulle relazioni di trust tra domini e foreste relative all'autenticazione, vedere [autenticazione delegata e relazioni di trust](https://technet.microsoft.com/library/dn169022.aspx).

## <a name="BKMK_CertificatesInWindowsAuthentication"></a>Certificati nell'autenticazione di Windows
Un'infrastruttura a chiave pubblica (PKI) è la combinazione di software, tecnologie di crittografia, processi e servizi che consentono a un'organizzazione di proteggere le comunicazioni e le transazioni aziendali. La capacità di un'infrastruttura a chiave pubblica per proteggere le comunicazioni e le transazioni aziendali è basata sullo scambio di certificati digitali tra gli utenti autenticati e le risorse attendibili.

Un certificato digitale è un documento elettronico che contiene informazioni sull'entità a cui appartiene, l'entità da cui è stato emesso, un numero di serie univoco o altre identificazioni univoche, date di rilascio e di scadenza e un'impronta digitale digitale.

L'autenticazione è il processo che consente di determinare se un host remoto può essere considerato attendibile. Per stabilirne l'attendibilità, l'host remoto deve fornire un certificato di autenticazione accettabile.

Gli host remoti stabiliscono la loro affidabilità ottenendo un certificato da un'autorità di certificazione (CA). La CA può, a sua volta, avere la certificazione di un'autorità superiore, che crea una catena di trust. Per determinare se un certificato è attendibile, un'applicazione deve determinare l'identità della CA radice e quindi determinare se è attendibile.

Analogamente, l'host remoto o il computer locale deve determinare se il certificato presentato dall'utente o dall'applicazione è autentico. Il certificato presentato dall'utente tramite LSA e SSPI viene valutato per l'autenticità nel computer locale per l'accesso locale, sulla rete o nel dominio tramite gli archivi certificati in Active Directory.

Per produrre un certificato, i dati di autenticazione passano attraverso gli algoritmi hash, ad esempio Secure Hash Algorithm 1 (SHA1), per produrre un digest del messaggio. Il digest del messaggio viene quindi firmato digitalmente utilizzando la chiave privata del mittente per dimostrare che il digest del messaggio è stato prodotto dal mittente.

> [!NOTE]
> SHA1 è l'impostazione predefinita in Windows 7 e Windows Vista, ma è stata modificata in SHA2 in Windows 8.

**Autenticazione con smart card**

La tecnologia Smart Card è un esempio di autenticazione basata su certificati. L'accesso a una rete con una smart card fornisce una forma avanzata di autenticazione perché usa l'identificazione basata sulla crittografia e la prova di possesso durante l'autenticazione di un utente in un dominio. Active Directory Servizi certificati (AD CS) fornisce l'identificazione basata su crittografia tramite il rilascio di un certificato di accesso per ogni smart card.

Per informazioni sull'autenticazione con smart card, vedere il [riferimento tecnico per le smart card di Windows](https://technet.microsoft.com/library/ff404297.aspx).

La tecnologia smart card virtuale è stata introdotta in Windows 8. Archivia il certificato della smart card nel PC e quindi lo protegge usando il chip di sicurezza del Trusted Platform Module di prova del dispositivo (TPM). In questo modo, il PC diventa effettivamente la smart card che deve ricevere il PIN dell'utente per poter essere autenticato.

**Autenticazione remota e wireless**

L'autenticazione di rete remota e wireless è un'altra tecnologia che usa i certificati per l'autenticazione. Il servizio di autenticazione Internet (IAS) e i server di rete privata virtuale utilizzano il protocollo EAP-TLS (Extensible Authentication Protocol-Transport Level Security), PEAP (Protected Extensible Authentication Protocol) o Internet Protocol Security (IPsec) a eseguire l'autenticazione basata sui certificati per molti tipi di accesso alla rete, tra cui la rete privata virtuale (VPN) e le connessioni wireless.

Per informazioni sull'autenticazione basata su certificati in rete, vedere [autenticazione e certificati di accesso alla rete](https://technet.microsoft.com/library/cc759575(WS.10).aspx).

## <a name="BKMK_SeeAlso"></a>Vedere anche
[Concetti su Autenticazione di Windows](https://docs.microsoft.com/windows-server/security/windows-authentication/windows-authentication-concepts)


