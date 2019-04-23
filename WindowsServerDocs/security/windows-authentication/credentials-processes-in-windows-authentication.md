---
title: Processi di credenziali in Autenticazione di Windows
description: Sicurezza di Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839862"
---
# <a name="credentials-processes-in-windows-authentication"></a>Processi di credenziali in Autenticazione di Windows

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento di riferimento per i professionisti IT descrive le modalità di elaborazione delle credenziali di autenticazione di Windows.

Gestione delle credenziali di Windows è il processo mediante il quale il sistema operativo riceve le credenziali dal servizio o utente e consente di proteggere tali informazioni per la presentazione del futuro di destinazione che esegue l'autenticazione. Nel caso di un computer aggiunto al dominio, la destinazione che esegue l'autenticazione è il controller di dominio. Le credenziali utilizzate nell'autenticazione sono documenti digitali che consentono di associare l'identità dell'utente in una forma di verifica dell'autenticità, ad esempio un certificato, una password o un PIN.

Per impostazione predefinita, le credenziali di Windows vengono convalidate sul database di gestione di account di sicurezza (SAM) nel computer locale o in Active Directory in un computer aggiunto al dominio, tramite il servizio Winlogon. Le credenziali vengono raccolte tramite input dell'utente nell'interfaccia utente di accesso o a livello di codice tramite l'interfaccia di programmazione dell'applicazione (API) devono essere presentati nella destinazione che esegue l'autenticazione.

Informazioni di sicurezza locali vengono archiviate nel Registro di sistema **HKEY_LOCAL_MACHINE\SECURITY**. Le informazioni archiviate includono impostazioni dei criteri, i valori di sicurezza predefiniti e informazioni sull'account, ad esempio le credenziali di accesso memorizzato nella cache. Una copia del database SAM anche viene archiviata in questo caso, sebbene sia protetto da scrittura.

Il diagramma seguente mostra i componenti necessari e i percorsi che richiedere le credenziali tramite il sistema per autenticare l'utente o il processo per un accesso corretto.

![Diagramma che mostra i componenti necessari e i percorsi che richiedere le credenziali tramite il sistema per autenticare l'utente o il processo per un accesso corretto.](../media/credentials-processes-in-windows-authentication/AuthN_LSA_Architecture_Client.gif)

La tabella seguente descrive ogni componente che gestisce le credenziali nel processo di autenticazione nel punto di accesso.

**Componenti di autenticazione per tutti i sistemi**

|Component|Descrizione|
|-------|--------|
|Accesso dell'utente|Winlogon.exe è il file eseguibile responsabile della gestione delle interazioni dell'utente protetta. Il servizio Winlogon avvia il processo di accesso per i sistemi operativi Windows, passando le credenziali raccolte dall'azione dell'utente sul desktop sicuro (interfaccia utente di accesso) per la sicurezza autorità locale (LSA) tramite Secur32.dll.|
|Accesso all'applicazione|L'applicazione o gli accessi di servizio che non richiedono l'accesso interattivo. Eseguire la maggior parte dei processi avviati dall'utente in modalità utente con Secur32.dll mentre i processi avviati all'avvio, ad esempio servizi, eseguito in modalità kernel usando Ksecdd.sys.<br /><br />Per altre informazioni sulla modalità utente e kernel, vedere le applicazioni e modalità utente o servizi e in modalità Kernel in questo argomento.|
|Secur32.dll|I provider di autenticazione più che costituiscono la base del processo di autenticazione.|
|Lsasrv.dll|Il servizio Server LSA, che consente di applicare i criteri di sicurezza e funge da Gestione pacchetto di sicurezza per LSA. LSA contiene la funzione Negotiate, che consente di selezionare il protocollo NTLM o Kerberos dopo aver determinato quale protocollo deve essere corretta.|
|Security Support Provider|Un set di provider che può richiamare singolarmente uno o più protocolli di autenticazione. Il set predefinito di provider è possibile modificare con ogni versione del sistema operativo Windows e i provider personalizzati possono essere scritti.|
|Netlogon.dll|I servizi che esegue il servizio Accesso rete sono come segue:<br /><br />-Mantiene canale sicuro del computer (non deve essere confuso con Schannel) a un controller di dominio.<br />-Passa le credenziali dell'utente tramite un canale sicuro per il controller di dominio e restituisce il dominio SID (security Identifier) e i diritti utente per l'utente.<br />-Pubblica i record di risorse del servizio nel sistema DNS (Domain Name) e Usa DNS per risolvere i nomi per gli indirizzi IP (Internet Protocol) dei controller di dominio.<br />-Implementa il protocollo di replica basato su chiamata di procedura remota (RPC) per la sincronizzazione dei controller di dominio primario (PDC) e il controller di dominio di backup (BDC).|
|Samsrv.dll|Il Security Account Manager (SAM), che archivia gli account di sicurezza locali, applica i criteri archiviati localmente e supporta le API.|
|Registro di sistema|Il Registro di sistema contiene una copia del database SAM, impostazioni di criteri di sicurezza locali, i valori di sicurezza predefiniti e informazioni sull'account accessibile solo al sistema.|

Questo argomento è suddiviso nelle sezioni seguenti:

-   [Input di credenziali per l'accesso utente](#BKMK_CrentialInputForUserLogon)

-   [Input di credenziali per l'accesso al servizio e dell'applicazione](#BKMK_CredentialInputForApplicationAndServiceLogon)

-   [Autorità di sicurezza locale](#BKMK_LSA)

-   [Convalida e credenziali memorizzate nella cache](#BKMK_CachedCredentialsAndValidation)

-   [Convalida e archiviazione delle credenziali](#BKMK_CredentialStorageAndValidation)

-   [Database di gestione degli account di sicurezza](#BKMK_SAM)

-   [I domini locali e dei domini trusted](#BKMK_LocalDomainsAndTrustedDomains)

-   [Certificati di autenticazione basata su Windows](#BKMK_CertificatesInWindowsAuthentication)

## <a name="BKMK_CrentialInputForUserLogon"></a>Input di credenziali per l'accesso utente
In Windows Server 2008 e Windows Vista, l'architettura Graphical Identification and Authentication (GINA) è stato sostituito con un modello di provider di credenziali, che è stato possibile enumerare i diversi tipi di accesso tramite l'uso di riquadri di accesso. Di seguito vengono descritti entrambi i modelli.

**Architettura con interfaccia grafica identificazione e autenticazione**

L'architettura Graphical Identification and Authentication (GINA) si applica ai sistemi operativi Windows Server 2003, Microsoft Windows 2000 Server, Windows XP e Windows 2000 Professional. In questi sistemi, ogni sessione di accesso interattivo consente di creare un'istanza separata del servizio Winlogon. L'architettura di GINA viene caricato nello spazio di processo usato da Winlogon, riceve ed elabora le credenziali ed effettua le chiamate alle interfacce di autenticazione tramite LSALogonUser.

Le istanze di Winlogon per un accesso interattivo vengono eseguite nella sessione 0. Servizi di sistema di host sessione 0 e gli altri processi critici, incluso il processo dell'autorità di protezione locale (LSA).

Il diagramma seguente illustra il processo di credenziali per Windows Server 2003, Microsoft Windows 2000 Server, Windows XP e Microsoft Windows 2000 Professional.

![Diagramma che illustra il processo di credenziali per Windows Server 2003, Microsoft Windows 2000 Server, Windows XP e Microsoft Windows 2000 Professional](../media/credentials-processes-in-windows-authentication/AuthN_GINA_Architecture.gif)

**Architettura del provider di credenziali**

Si applica l'architettura di provider di credenziali per le versioni indicate nel **si applica a** elenco all'inizio di questo argomento. In questi sistemi, l'architettura di input per le credenziali modificato in una progettazione estensibile tramite i provider di credenziali. Questi provider sono rappresentati dai riquadri di accesso diverso sul desktop sicuro che consentono a qualsiasi numero di scenari di accesso - account diversi per lo stesso utente e i metodi di autenticazione diversi, ad esempio password, smart card e biometria.

Con l'architettura di provider di credenziali, Winlogon avvia sempre dell'interfaccia utente di accesso dopo la ricezione di un evento di sequenza attenzione sicura. Interfaccia utente di accesso esegue una query ogni provider di credenziali per il numero di diversi tipi di credenziali che è configurato il provider per enumerare. I provider di credenziali hanno la possibilità di specificare uno di questi riquadri come impostazione predefinita. Dopo che tutti i provider sono enumerati i riquadri, interfaccia utente di accesso li visualizza all'utente. L'utente interagisce con un riquadro per fornire le proprie credenziali. Interfaccia utente di accesso invia le credenziali per l'autenticazione.

I provider di credenziali non sono meccanismi di imposizione. Vengono utilizzati per raccogliere e la serializzazione delle credenziali. I pacchetti di autorità di sicurezza locale e l'autenticazione di applicano la sicurezza.

I provider di credenziali sono registrati nel computer e sono responsabili per gli elementi seguenti:

-   Che descrive le informazioni sulle credenziali necessarie per l'autenticazione.

-   Gestione delle comunicazioni e per la logica con le autorità di autenticazione esterni.

-   Le credenziali di creazione di pacchetti per interattiva e accesso alla rete.

Le credenziali di creazione di pacchetti per interattiva e accesso alla rete include il processo di serializzazione. Per la serializzazione delle credenziali più riquadri di accesso possono essere visualizzati nell'account di accesso dell'interfaccia utente. Di conseguenza, l'organizzazione può controllare la visualizzazione di accesso, ad esempio gli utenti, i sistemi di destinazione per l'accesso, pre-logon accesso di rete e la workstation Blocca/Sblocca i criteri - tramite l'uso di provider di credenziali personalizzate. Più provider di credenziali possono coesistere nello stesso computer.

Single sign-on (SSO) i provider possono essere sviluppati come un provider di credenziali standard o come un Provider di Pre-Logon-Access.

Ogni versione di Windows contiene il provider di credenziali di un valore predefinito e un valore predefinito Pre-Logon-Access Provider (PLAP), noto anche come il provider SSO. Il provider SSO consente agli utenti di effettuare una connessione a una rete prima dell'accesso al computer locale. Quando questo provider viene implementato, il provider non enumera i riquadri nell'interfaccia utente di accesso.

Un provider SSO dovrà essere utilizzato negli scenari seguenti:

-   **Accesso alla rete computer e di autenticazione vengono gestite dai provider di credenziali diverso.** Le variazioni a questo scenario includono:

    -   Un utente ha la possibilità di connettersi a una rete, ad esempio la connessione a una rete privata virtuale (VPN), prima di accedere al computer ma non è necessario stabilire una connessione.

    -   L'autenticazione di rete è necessario per recuperare le informazioni utilizzate durante l'autenticazione interattiva nel computer locale.

    -   Più autenticazioni di rete sono seguite da uno degli altri scenari. Ad esempio, un utente esegue l'autenticazione a un provider di servizi Internet (ISP), esegue l'autenticazione in una rete privata virtuale e quindi utilizza le credenziali dell'account utente per l'accesso in locale.

    -   Credenziali memorizzate nella cache sono disabilitate ed è necessaria una connessione di servizi di accesso remoto tramite VPN prima dell'accesso locale per autenticare l'utente.

    -   Un utente di dominio non è un account locale impostare in un computer aggiunto al dominio e deve essere stabilita una connessione di servizi di accesso remoto tramite la connessione VPN prima di completare l'accesso interattivo.

-   **Accesso alla rete computer e di autenticazione vengono gestite dallo stesso provider di credenziali.** In questo scenario, l'utente è obbligatorio per la connessione alla rete prima di accedere al computer.

**Enumerazione riquadro di accesso**

Il provider di credenziali enumera i riquadri di accesso nei casi seguenti:

-   Per tali sistemi operativi indicati nel **si applica a** elenco all'inizio di questo argomento.

-   Il provider di credenziali enumera i riquadri per l'accesso a una workstation. Il provider di credenziali è in genere serializza le credenziali per l'autenticazione all'autorità di sicurezza locale. Questo processo consente di visualizzare i riquadri specifici per ogni utente e i sistemi di destinazione di ogni utente.

-   L'architettura di accesso e autenticazione consente a un utente di usare i riquadri enumerati tramite il provider di credenziali per sbloccare una workstation. In genere, l'utente attualmente connesso è il riquadro predefinito, ma se più di un utente è connesso, vengono visualizzati i riquadri diversi.

-   Il provider di credenziali enumera i riquadri in risposta a una richiesta dell'utente per modificare la password o altre informazioni private, ad esempio un PIN. In genere, l'utente attualmente connesso è il riquadro predefinito; Tuttavia, se più di un utente è connesso, vengono visualizzati numerosi riquadri.

-   Il provider di credenziali enumera i riquadri basati su quelle serializzati da utilizzare per l'autenticazione nei computer remoti. Le credenziali dell'interfaccia utente non usano la stessa istanza del provider come l'interfaccia utente di accesso, le Workstation sbloccare o modificare la Password. Di conseguenza, le informazioni sullo stato non è possibile gestire nel provider tra le istanze dell'interfaccia utente delle credenziali. Questa struttura comporta un riquadro per ogni accesso al computer remoto, presupponendo che le credenziali sono state serializzate in modo corretto. Questo scenario viene usato anche in controllo Account utente (UAC), che possono aiutare a evitare modifiche non autorizzate a un computer da cui viene richiesto all'utente l'autorizzazione o una password di amministratore prima di consentire le azioni che potrebbero influire sul funzionamento del computer o che è stato possibile modificare le impostazioni che influiscono su altri utenti del computer.

Il diagramma seguente illustra il processo di credenziali per i sistemi operativi indicati nel **si applica a** elenco all'inizio di questo argomento.

![Diagramma che illustra il processo di credenziali per i sistemi operativi indicati nella * * si applica a * * elenco all'inizio di questo argomento](../media/credentials-processes-in-windows-authentication/AuthN_CredMan_CredProv.gif)

## <a name="BKMK_CredentialInputForApplicationAndServiceLogon"></a>Input di credenziali per l'accesso al servizio e dell'applicazione
L'autenticazione di Windows è progettato per gestire le credenziali per le applicazioni o servizi che non richiedono l'interazione dell'utente. Le applicazioni in modalità utente sono limitate a quali risorse di sistema potranno accedervi, mentre i servizi possono avere accesso illimitato per la memoria di sistema e dispositivi esterni.

Servizi di sistema e le applicazioni a livello di trasporto accedono un Security Support Provider (SSP) tramite sicurezza Support Provider Interface (SSPI) in Windows, che fornisce funzioni per l'enumerazione dei pacchetti di sicurezza disponibili in un sistema, la selezione di un pacchetto e utilizzo di tale pacchetto per ottenere una connessione autenticata.

Quando una connessione client/server viene autenticata:

-   L'applicazione sul lato client della connessione invia le credenziali al server tramite la funzione SSPI `InitializeSecurityContext (General)`.

-   L'applicazione sul lato server della connessione risponde con la funzione SSPI `AcceptSecurityContext (General)`.

-   Le funzioni SSPI `InitializeSecurityContext (General)` e `AcceptSecurityContext (General)` vengono ripetuti fino a quando non sono stati scambiati tutti i messaggi di autenticazione necessarie per l'esito positivo o negativo di autenticazione.

-   Dopo la connessione è stata autenticata, LSA sul server usa le informazioni dal client per creare il contesto di sicurezza, che contiene un token di accesso.

-   Il server può quindi chiamare la funzione SSPI `ImpersonateSecurityContext` per associare il token di accesso a un thread di rappresentazione per il servizio.

**Le applicazioni e modalità utente**

Modalità utente in Windows è costituita da due sistemi in grado di passare le richieste dei / o il driver in modalità kernel appropriato: il sistema di ambiente, che esegue le applicazioni scritte per molti tipi diversi di sistemi operativi, e il sistema integrale, che opera funzioni specifiche del sistema per conto del sistema di ambiente.

Il sistema integrale gestisce alcune funzioni system'specific operative per conto del sistema di ambiente e costituito da un processo di sistema di sicurezza (LSA), un servizio workstation e un servizio del server. Il processo di sistema di sicurezza riguarda i token di sicurezza, concede o Nega le autorizzazioni per accedere agli account utente in base alle autorizzazioni di risorse, gestisce le richieste di accesso e avvia l'autenticazione di accesso e determina quali risorse di sistema del sistema operativo è necessario controllare.

Le applicazioni eseguibili in modalità utente, in cui eseguire l'applicazione, in quanto le entità incluse nel contesto di sicurezza di sistema locale (SYSTEM). Le applicazioni possono anche eseguire in modalità kernel in cui l'applicazione può essere eseguita nel contesto di sicurezza di sistema locale (SYSTEM).

SSPI è disponibile tramite il modulo Secur32.dll, che è un'API utilizzata per ottenere i servizi di sicurezza integrata per l'autenticazione, l'integrità e riservatezza dei messaggi. Fornisce un livello di astrazione tra i protocolli a livello di applicazione e i protocolli di sicurezza. Poiché applicazioni diverse richiedono diverse modalità di identificazione o autenticazione di utenti e crittografia dei dati durante il trasferimento in una rete in diversi modi, SSPI fornisce un modo per accedere alle librerie a collegamento dinamico (DLL) che contengono l'autenticazione diversa e funzioni di crittografia. Queste DLL sono chiamate Security Support Provider (SSP).

Account del servizio gestiti e account virtuali sono state introdotte in Windows Server 2008 R2 e Windows 7 per offrire ad applicazioni importanti, ad esempio Microsoft SQL Server e Internet Information Services (IIS), l'isolamento dei relativi account di dominio, mentre eliminando la necessità di un amministratore amministri manualmente il nome dell'entità servizio (SPN) e le credenziali per questi account. Per altre informazioni su queste funzionalità e il loro ruolo con l'autenticazione, vedere [Managed Service account documentazione per Windows 7 e Windows Server 2008 R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx) e [panoramica degli account del servizio gestito gruppo](../group-managed-service-accounts/group-managed-service-accounts-overview.md).

**Modalità kernel e servizi**

Anche se la maggior parte delle applicazioni di Windows vengono eseguiti nel contesto di sicurezza dell'utente che avvia, ciò non vale per i servizi. Molti servizi di Windows, ad esempio rete e servizi di stampa, vengono avviati dal controller di servizio quando l'utente avvia il computer. Questi servizi potrebbe essere eseguito come servizio locale o sistema locale e possono rimanere in esecuzione dopo l'ultimo utente si disconnette.

> [!NOTE]
> Servizi in genere eseguiti in contesti di sicurezza noti come sistema locale (SYSTEM), servizio di rete o servizio locale.  Windows Server 2008 R2 ha introdotto servizi eseguiti con un account del servizio gestito, che sono entità di dominio.

Prima di avviare un servizio, il controller del servizio esegue l'accesso usando l'account che è designato per il servizio e quindi presenta le credenziali del servizio di autenticazione tramite autorità di protezione locale. Il servizio Windows implementa un'interfaccia programmatica che è possibile usare Gestione controllo servizi per controllare il servizio. Un servizio di Windows può essere avviato automaticamente quando viene avviato il sistema o manualmente tramite un programma di controllo del servizio. Ad esempio, quando un computer client Windows viene aggiunto a un dominio, il servizio messenger sul computer si connette a un controller di dominio e viene aperto un canale sicuro a esso. Per ottenere una connessione autenticata, il servizio deve disporre delle credenziali che considera attendibile l'autorità di sicurezza locale (LSA del computer remoto). Quando si comunica con altri computer nella rete, LSA utilizza le credenziali per l'account di dominio del computer locale, come tutti gli altri servizi in esecuzione nel contesto di sicurezza del sistema locale e del servizio di rete. Servizi nel computer locale eseguito come sistema in modo che le credenziali non devono essere presenti per LSA.

Il file Ksecdd.sys gestisce e consente di crittografare le credenziali e Usa una chiamata di procedura locale in LSA. Il tipo di file è DRV (driver) ed è noto come Security Support Provider (SSP) la modalità kernel e, in tali versioni indicate nel **si applica a** elencati all'inizio di questo argomento, è compatibile con FIPS 140-2 livello 1-conforme.

Modalità kernel ha accesso completo alle risorse hardware e del sistema del computer. La modalità kernel arresta i servizi in modalità utente e le applicazioni potranno accedere alle sezioni critiche del sistema operativo che non si dovrebbero avere accesso a.

## <a name="BKMK_LSA"></a>Autorità di sicurezza locale
L'autorità di sicurezza locale (LSA) è un processo di sistema protetto che autentica e registra gli utenti al computer locale. Inoltre, LSA gestisce le informazioni su tutti gli aspetti di sicurezza locale in un computer (questi aspetti sono noti collettivamente come criterio di protezione locale) e offre diversi servizi per la conversione tra i nomi e ID di sicurezza (SID). Il processo di sistema di sicurezza, protezione Server servizio LSASS (Local Authority), tiene traccia dei criteri di sicurezza e gli account che sono in effetti in un computer.

LSA convalida un'identità dell'utente in base a quale delle due seguenti entità rilasciata l'account dell'utente:

-   **Autorità di sicurezza locale.** LSA possibile convalidare le informazioni utente dal controllo del database di gestione di account di sicurezza (SAM) che si trova nello stesso computer. Qualsiasi workstation o server membro possono archiviare informazioni sui gruppi locali e account utente locali. Tuttavia, questi account sono utilizzabile per l'accesso solo tale workstation o computer.

-   **Autorità di sicurezza per il dominio locale o per un dominio trusted.** LSA contatta l'entità che ha emesso l'account e richiede di verificare che l'account sia valido e che ha avuto origine la richiesta dal titolare dell'account.

Il servizio LSASS (Local Security Authority Subsystem Service) permette di archiviare credenziali in memoria per conto di utenti con sessioni attive di Windows. Le credenziali archiviate consentono agli utenti di accedere facilmente alle risorse di rete, ad esempio condivisioni file, cassette postali di Exchange Server e siti di SharePoint, senza dover immettere nuovamente le proprie credenziali per ogni servizio remoto.

Il servizio LSASS permette di archiviare credenziali in più formati, inclusi i seguenti:

-   Testo normale crittografato in modo reversibile

-   Ticket Kerberos (ticket-granting ticket (TGT), ticket di servizio)

-   Hash NT

-   Hash di LAN Manager (LM)

Se l'utente accede a Windows usando una smart card, LSASS non archivia una password non crittografata, ma archivia il valore hash NT corrispondente per l'account e il testo non crittografato PIN della smart card. Se l'attributo dell'account è abilitato per una smart card necessaria per l'accesso interattivo, un valore di hash NT casuale viene generato automaticamente per l'account invece dell'hash della password originale. L'hash di password generato automaticamente quando si imposta l'attributo non viene modificato.

Se un utente accede a un computer basato su Windows con una password che è compatibile con gli hash di LAN Manager (LM), questo autenticatore è presente in memoria.

L'archiviazione di credenziali in testo non crittografato in memoria non può essere disabilitata, anche se i provider di credenziali che le richiedono sono disabilitati.

Le credenziali archiviate sono associate direttamente le sessioni di accesso LSASS Local Security Authority Subsystem Service () che sono state avviate dopo l'ultimo riavvio e non sono stati chiusi. Ad esempio, sessioni LSA con credenziali LSA archiviate vengono create quando un utente esegue una delle operazioni seguenti:

-   Accede a una sessione locale o Remote Desktop Protocol (RDP) nel computer

-   Esecuzione di un task con l'opzione **RunAs**

-   Esecuzione di un servizio di Windows attivo nel computer

-   Esecuzione di un'attività o un processo batch pianificato

-   Esecuzione di un'attività nel computer locale tramite uno strumento di amministrazione remota

In alcuni casi, i segreti LSA, che sono segrete porzioni di dati che sono accessibili solo ai processi di account di sistema, vengono archiviati nell'unità disco rigido. Alcuni di questi segreti sono credenziali che devono essere mantenute dopo un riavvio e sono archiviate in formato crittografato nell'unità disco rigido. Le credenziali archiviate come segreti LSA possono includere:

-   Password dell'account per l'account del computer Active Directory Domain Services (AD DS)

-   Password di account per servizi di Windows configurati nel computer

-   Password di account per le attività pianificate configurate

-   Password di account per pool di applicazioni IIS e siti Web

-   Password degli account Microsoft

Introdotta in Windows 8.1, il sistema operativo client offre protezione aggiuntiva per LSA per impedire la lettura e inserimento di codice da processi non protetti. Questa protezione aumenta la protezione per le credenziali che archivia e gestisce di LSA.

Per altre informazioni su queste protezioni aggiuntive, vedere [Configuring Additional LSA Protection](../credentials-protection-and-management/configuring-additional-lsa-protection.md).

## <a name="BKMK_CachedCredentialsAndValidation"></a>Convalida e credenziali memorizzate nella cache
Meccanismi di convalida si basano sulla presentazione preparata e delle credenziali al momento dell'accesso. Tuttavia, quando il computer è disconnesso da un controller di dominio e l'utente sta presentando le credenziali di dominio, Windows Usa il processo di credenziali memorizzate nella cache del meccanismo di convalida.

Ogni volta che un utente accede a un dominio Windows memorizza nella cache le credenziali fornite e li archivia nell'oggetto hive di sicurezza nel Registro di sistema del sistema operativo.

Con le credenziali memorizzate nella cache, l'utente può accedere a un membro di dominio senza connettersi a un controller di dominio all'interno del dominio.


## <a name="BKMK_CredentialStorageAndValidation"></a>Convalida e archiviazione delle credenziali
Non è sempre opportuno utilizzare un insieme di credenziali per l'accesso a risorse diverse. Ad esempio, un amministratore potrebbe desidera utilizzare Amministrazione anziché le credenziali dell'utente quando si accede a un server remoto. Analogamente, se un utente accede a risorse esterne, ad esempio un conto bancario, direste possono solo usare credenziali diverse rispetto a quelle le credenziali del dominio. Le sezioni seguenti descrivono le differenze nella gestione delle credenziali tra le versioni correnti di sistemi operativi Windows e i sistemi operativi Windows Vista e Windows XP.

### <a name="remote-logon-credential-processes"></a>Processi di credenziali di accesso remoto
Remote Desktop Protocol (RDP) gestisce le credenziali dell'utente che si connette a un computer remoto usando il Client Desktop remoto, introdotto in Windows 8. Le credenziali in formato non crittografato vengono inviate all'host di destinazione in cui l'host tenta di eseguire il processo di autenticazione e, se l'operazione riesce, l'utente si connette alle risorse consentite. RDP non archivia le credenziali sul client, ma le credenziali di dominio dell'utente vengono archiviate in LSASS.

Introdotta in Windows Server 2012 R2 e Windows 8.1, modalità di amministrazione limitata offre sicurezza aggiuntiva per scenari di accesso remoto. Questa modalità di Desktop remoto, l'applicazione client eseguire un'accesso richiesta-risposta di rete con la funzione unidirezionale NT (NTOWF) o utilizzare un ticket di servizio Kerberos per l'autenticazione all'host remoto. Dopo aver autenticato l'amministratore, l'amministratore non è le credenziali dell'account corrispondente in LSASS poiché non sono stati forniti all'host remoto. Al contrario, l'amministratore ha le credenziali dell'account computer per la sessione. Le credenziali di amministratore non vengono fornite all'host remoto, pertanto le azioni vengono eseguite come account del computer. Le risorse sono anche limitate all'account del computer e l'amministratore non può accedere alle risorse con il proprio account.

### <a name="automatic-restart-sign-on-credential-process"></a>Il riavvio automatico sign-on di elaborazione delle credenziali
Quando un utente accede in un dispositivo Windows 8.1, LSA Salva le credenziali dell'utente in memoria crittografata che sono accessibili solo dal LSASS.exe. Quando Windows Update avvia un'operazione di riavvio automatico senza presenza utente, queste credenziali vengono usate per configurare l'accesso automatico per l'utente.

Al riavvio, l'utente viene automaticamente effettuato l'accesso tramite il meccanismo di accesso automatico e quindi il computer viene inoltre bloccato per proteggere la sessione dell'utente. Il blocco viene avviato tramite Winlogon mentre la gestione delle credenziali avviene tramite LSA. Eseguendo l'accesso automaticamente e il blocco della sessione dell'utente sulla console, alle applicazioni schermata di blocco dell'utente è riavviate e disponibili.

Per altre informazioni sulle ARSO, vedere [Winlogon automatica riavviare Sign-On &#40;ARSO&#41;](winlogon-automatic-restart-sign-on-arso.md).

### <a name="stored-user-names-and-passwords-in-windows-vista-and-windows-xp"></a>Gestione nomi utente e password in Windows Vista e Windows XP
In Windows Server 2008, Windows Server 2003, Windows Vista e Windows XP **archiviati nomi utente e password** nel Pannello di controllo consente di semplificare la gestione e l'uso di più insiemi di credenziali di accesso, inclusi i certificati X.509 usati con smart card e le credenziali di Windows Live (ora denominate account Microsoft). -Parte del profilo dell'utente: le credenziali vengono archiviate fino a quando necessario. Questa azione può aumentare la sicurezza una base alle risorse, garantendo che se una password è compromesso, non comprometta tutta la sicurezza.

Dopo che un utente accede e tenta di accedere a risorse aggiuntive protetto da password, ad esempio una condivisione in un server, e se le credenziali di accesso predefinite dell'utente non sono sufficienti per ottenere l'accesso **archiviati nomi utente e password** viene eseguita una query . Se sono state salvate credenziali alternative con le informazioni di accesso corretto nelle **archiviati nomi utente e password**, queste credenziali vengono usate per ottenere l'accesso. In caso contrario, l'utente viene richiesto di fornire nuove credenziali, che possono essere quindi salvate per il riutilizzo, in un secondo momento nella sessione di accesso o durante una sessione successiva.

Si applicano le restrizioni seguenti:

-   Se **archiviati nomi utente e password** contiene le credenziali non valide o non corrette per una risorsa specifica, l'accesso alla risorsa viene negata e il **archiviati nomi utente e password** non la finestra di dialogo vengono visualizzati.

-   **I nomi utente e password archiviati** archivia le credenziali solo per protocollo Kerberos, NTLM, account Microsoft (precedentemente Windows Live ID) e l'autenticazione di Secure Sockets Layer (SSL). Alcune versioni di Internet Explorer mantengono le proprie cache per l'autenticazione di base.

Queste credenziali diventano una parte del profilo locale dell'utente nella directory Settings\NomeUtente\Application Data\Microsoft\Credentials \Documents and crittografata. Di conseguenza, queste credenziali possono effettuare il roaming con l'utente se criteri di rete dell'utente supportano i profili utente mobili. Tuttavia, se l'utente ha copie dei **archiviati nomi utente e password** su due computer differenti e modifiche le credenziali che sono associate con la risorsa in uno di questi computer, la modifica non viene propagata al  **I nomi utente e password archiviati** nel secondo computer.

### <a name="windows-vault-and-credential-manager"></a>Insieme di credenziali di Windows e gestione credenziali
Gestione credenziali è stato introdotto in Windows Server 2008 R2 e Windows 7 come funzionalità del Pannello di controllo per archiviare e gestire nomi utente e password. Gestione credenziali consente agli utenti di archiviare le credenziali rilevanti per altri sistemi e siti Web nell'insieme di credenziali di Windows protetto. Alcune versioni di Internet Explorer usano questa funzionalità per l'autenticazione per siti Web.

La gestione delle credenziali mediante Gestione credenziali è controllata dall'utente nel computer locale. Gli utenti possono salvare e archiviare le credenziali dai browser supportati e dalle applicazioni Windows in modo da rendere più pratico l'accesso a queste risorse. Le credenziali vengono salvate in speciali cartelle crittografate nel computer nel profilo dell'utente. Le applicazioni che supportano questa funzionalità (utilizzando l'API di gestione credenziali), ad esempio i browser web e App, possono presentare le credenziali corrette ad altri computer e siti Web durante il processo di accesso.

Quando un sito Web, un'applicazione o un altro computer richiede l'autenticazione tramite il protocollo Kerberos o NTLM, una finestra di dialogo viene visualizzata in cui si seleziona il **Aggiorna credenziali predefinite** o **Salva Password**casella di controllo. Questa finestra di dialogo che consente agli utenti di salvare le credenziali in locale viene generata da un'applicazione che supporta le API di gestione credenziali. Se l'utente seleziona il **Salva Password** casella di controllo, Gestione credenziali tiene traccia di nome utente dell'utente, password e informazioni correlate per il servizio di autenticazione in uso.

La volta successiva che viene usato il servizio, Gestione credenziali fornisce automaticamente le credenziali che viene archiviata nell'insieme di credenziali di Windows. Se queste non vengono accettate, le informazioni di accesso corrette vengono richieste all'utente. Se viene concesso l'accesso con le nuove credenziali, Gestione credenziali sovrascrivono le precedenti con quello nuovo e quindi Archivia le nuove credenziali nell'insieme di credenziali di Windows.

## <a name="BKMK_SAM"></a>Database di gestione degli account di sicurezza
La gestione di account di sicurezza (SAM) è un database che archivia i gruppi e account utente locali. È presente in ogni sistema operativo Windows; Tuttavia, quando un computer è unito a un dominio, Active Directory gestisce gli account di dominio nei domini Active Directory.

Ad esempio, i computer client che eseguono un sistema operativo Windows partecipano a un dominio di rete mediante la comunicazione con un controller di dominio anche quando non è connesso alcun utente umano. Per avviare le comunicazioni, il computer deve avere un account attivo del dominio. Prima di accettare le comunicazioni dal computer, LSA nel controller di dominio autentica l'identità del computer e quindi crea il contesto di sicurezza del computer esattamente come avviene per un'entità di sicurezza risorse umane. In questo contesto di sicurezza definisce le identità e le funzionalità di un utente o un servizio in un determinato computer o un utente, il servizio oppure computer in una rete. Ad esempio, il token di accesso contenuto all'interno del contesto di sicurezza definisce le risorse (ad esempio una condivisione file o una stampante) che è possibile accedere e le azioni che possono essere eseguite da tale entità - un utente, computer o servizio su esso (ad esempio lettura, scrittura o modifica) risorsa.

Il contesto di sicurezza di un utente o computer può variare da un computer a un altro, ad esempio quando un utente accede a un workstation o un server diverso da workstation primario dell'utente. Anche può variare da una sessione a altra, ad esempio quando un amministratore modifica i diritti e autorizzazioni dell'utente. Inoltre, il contesto di sicurezza è in genere diverso quando un utente o computer opera in modo autonomo, in una rete o come parte di un dominio di Active Directory.

## <a name="BKMK_LocalDomainsAndTrustedDomains"></a>I domini locali e dei domini trusted
Quando esiste una relazione di trust tra due domini, i meccanismi di autenticazione per ogni dominio si basano sulla validità dell'autenticazione di provenienti da altro dominio. Considera attendibile la Guida per fornire accesso controllato alle risorse condivise in un dominio di risorsa (il dominio trusting) verificando che l'autenticazione in ingresso richieste provenienza da un'autorità attendibile (il dominio trusted). In questo modo, i trust agiscono come bridge che consentano solo convalidata viaggio di richieste di autenticazione tra domini.

La modalità con cui una relazione di trust specifico passa le richieste di autenticazione dipende dal modo in cui è configurato. Relazioni di trust possono essere unidirezionale, consentendo l'accesso dal dominio trusted alle risorse nel dominio trusting o bidirezionale, fornendo l'accesso da ogni dominio alle risorse in altro dominio. Relazioni di trust sono inoltre non, nel qual caso non esiste una relazione di trust solo tra i domini di due trust partner, transitive o transitive, nel qual caso una relazione di trust esteso automaticamente ad altri domini trusting di entrambi i partner.

Per informazioni sulle relazioni di trust tra domini e foreste relative all'autenticazione, vedere [relazioni di Trust e dell'autenticazione delegata](https://technet.microsoft.com/library/dn169022.aspx).

## <a name="BKMK_CertificatesInWindowsAuthentication"></a>Certificati di autenticazione basata su Windows
Un'infrastruttura a chiave pubblica (PKI) è la combinazione di software, le tecnologie di crittografia, processi e servizi che consentono alle organizzazioni di proteggere le comunicazioni e le transazioni aziendali. La capacità di un'infrastruttura a chiave pubblica per proteggere le comunicazioni e le transazioni aziendali si basa sullo scambio di certificati digitali tra gli utenti autenticati e alle risorse attendibili.

Un certificato digitale è un documento elettronico contenente informazioni sull'entità che a cui appartiene l'entità da che sia stato emesso, un numero di serie univoco o alcuni altri identificazione univoca, rilascio e date di scadenza e un'impronta digitale.

L'autenticazione è il processo volto a determinare se un host remoto può essere considerato attendibile. Per stabilire la sua affidabilità, l'host remoto deve fornire un certificato di autenticazione accettabile.

Host remoti stabilire loro attendibilità ottenendo un certificato da un'autorità di certificazione (CA). L'autorità di certificazione possono, a sua volta, hanno la certificazione da un'autorità maggiore, che crea una catena di trust. Per determinare se un certificato è attendibile, un'applicazione deve determinare l'identità della CA radice e quindi determinare se è attendibile.

Analogamente, il computer locale o un host remoto deve determinare se il certificato presentato dall'utente o dell'applicazione è autentico. Il certificato presentato dall'utente tramite la LSA e SSPI viene valutato l'autenticità del computer locale per l'accesso locale, nella rete o nel dominio tramite gli archivi certificati in Active Directory.

Per generare un certificato, i dati di autenticazione passa attraverso gli algoritmi di hash, ad esempio Secure Hash Algorithm 1 (SHA1), per produrre un digest del messaggio. Quindi il digest del messaggio sono firmati digitalmente con chiave privata del mittente per dimostrare che il digest del messaggio è stato generato dal mittente.

> [!NOTE]
> SHA1 è l'impostazione predefinita in Windows 7 e Windows Vista, ma è stato modificato in SHA-2 in Windows 8.

**Autenticazione con smart card**

Tecnologia delle smart card è un esempio dell'autenticazione basata su certificato. L'accesso a una rete con una smart card rappresenta un tipo di autenticazione sicuro perché Usa basati su crittografia identificazione e la prova di possesso durante l'autenticazione di un utente a un dominio. Servizi certificati Active Directory (AD CS) fornisce l'identificazione di crittografia basate su attraverso il rilascio di un certificato di accesso per ogni smart card.

Per informazioni sull'autenticazione con smart card, vedere la [tecnica delle Smart Card Windows](https://technet.microsoft.com/library/ff404297.aspx).

La tecnologia smart card virtuale è stata introdotta in Windows 8. Archivia il certificato della smart card nel PC e quindi lo protegge con chip di sicurezza del dispositivo prova di manomissione modulo TPM (Trusted Platform). In questo modo, il PC diventa di fatto la smart card che devono ricevere il PIN dell'utente per poter essere autenticato.

**Autenticazione remota e wireless**

L'autenticazione di rete wireless e remoto è un'altra tecnologia che usa i certificati per l'autenticazione. Il Servizio autenticazione Internet (IAS) e i server di rete privata virtuale utilizzano Extensible Authentication Protocol-Transport Level Security (EAP-TLS), PEAP Protected Extensible Authentication Protocol () o Internet Protocol security (IPsec) per eseguire l'autenticazione basata su certificati per molti tipi di accesso alla rete, inclusi la rete privata virtuale (VPN) e le connessioni wireless.

Per informazioni sull'autenticazione basata su certificati in rete, vedere [certificati e autenticazione di accesso di rete](https://technet.microsoft.com/library/cc759575(WS.10).aspx).

## <a name="BKMK_SeeAlso"></a>Vedere anche
[Concetti di autenticazione di Windows](https://technet.microsoft.com/library/d169018.aspx)


