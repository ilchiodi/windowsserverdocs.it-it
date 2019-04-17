---
title: Concetti di autenticazione di Windows
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 29d1db15-cae0-4e3d-9d8e-241ac206bb8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 27e124c971926edd33f102fe6009a0c552c6d814
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="windows-authentication-concepts"></a>Concetti di autenticazione di Windows

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento di riferimento Panoramica descrive i concetti su cui si basa l'autenticazione di Windows.

L'autenticazione è un processo di verifica dell'identità di un oggetto o di una persona. Quando si autentica un oggetto, l'obiettivo consiste nel verificare che l'oggetto sia autentico. Quando si autentica una persona, l'obiettivo consiste nel verificare che l'utente non sia un impostore.

In un contesto di rete, l'autenticazione è l'atto di dimostrare l'identità a un'applicazione di rete o una risorsa. In genere, identità viene dimostrata tramite un'operazione di crittografia che utilizza una chiave solo l'utente sa (come crittografia a chiave pubblica) o una chiave condivisa. Sul lato server dello scambio di autenticazione confronta i dati firmati con una chiave crittografica nota per convalidare il tentativo di autenticazione.

Archiviare le chiavi di crittografia in una posizione centralizzata sicura rende il processo di autenticazione, scalabile e gestibile. Active Directory è consigliata e le chiavi di tecnologia predefinita per archiviare le informazioni di identità, che includono la crittografia sono le credenziali dell'utente. Active Directory è necessario per impostazione predefinita NTLM e Kerberos implementazioni.

Intervallo di tecniche di autenticazione da un semplice accesso a un sistema operativo o un accesso a un servizio o applicazione, che identifica gli utenti in base a qualcosa che solo l'utente conosce, ad esempio una password, a meccanismi di sicurezza più potenti che utilizzano informazioni possedute has'such l'utente come token, certificati di chiave pubblica, immagini o attributi biologici. In un ambiente aziendale, gli utenti possono accedere a più applicazioni su molti tipi di server all'interno di un'unica posizione o in diverse posizioni. Per questi motivi, l'autenticazione deve supportare ambienti per altre piattaforme e per altri sistemi operativi Windows.

## <a name="authentication-and-authorization-a-travel-analogy"></a>Autenticazione e autorizzazione: un'analogia viaggi
Un'analogia viaggi consentono di spiegare come funziona l'autenticazione. Alcune attività preliminari sono in genere necessarie per iniziare il viaggio. Il denaro necessario per deve dimostrare la propria identità true all'autorità di host. Questo modello di prova può essere sotto forma di prova di cittadinanza, luogo di nascita, un giustificativo personale, fotografie o quello è richiesto dalla legge del paese host. Identità del denaro di necessario per viene convalidato per il rilascio di un account passport, che è analogo a un account di sistema rilasciato e amministrati da un'organizzazione, l'entità di sicurezza. Passport e la destinazione sono basate su un set di regole e alle normative rilasciate dall'autorità governativa.

**L'avventura**

Quando raggiunge il bordo internazionale, il denaro necessario per una guardia border richiede le credenziali e il denaro necessario per presenta le proprie passport. Il processo è duplice:

-   La protezione autentica il passport, verificare che sia stato emesso da un'autorità di sicurezza che amministrazione locale trust (trust, almeno, per il rilascio di passaporti) e verificando che il passport non è stato modificato.

-   La protezione autentica il denaro necessario per verificando che il volto corrisponde la superficie della persona illustrata di passport e che altre credenziali necessari siano in buono stato.

Se passport dimostra sia valido e il denaro necessario per dimostra di essere il proprietario, l'autenticazione ha esito positivo e il denaro necessario per può essere consentito l'accesso tra il bordo.

Trust transitivo tra le autorità di sicurezza costituisce la base di autenticazione. il tipo di autenticazione che viene eseguita in un bordo internazionale si basa sull'attendibilità. Non conosce il denaro necessario per l'amministrazione locale, ma che il governo host non trusted. Quando l'host patenti di passport, non nota il denaro necessario per essere. Considerato attendibile agenzia che ha emesso il certificato di nascita o altra documentazione. L'ente che ha emesso il certificato di nascita, a sua volta, trusted medico che ha firmato il certificato. Il medico avvenire alla presenza di nascita il denaro del necessario per e contrassegnato in questo caso il certificato con la prova diretto dell'identità con footprint per. Trust che viene trasferito in questo modo, tramite intermediari attendibili, è transitivo.

Trust transitivo è la base per la sicurezza di rete in architettura client/server di Windows. Una relazione di trust è disposto in tutta una serie di domini, ad esempio un albero di dominio e stabilisce una relazione tra un dominio e tutti i domini trusting del dominio. Ad esempio, se il dominio dispone di un trust transitivo con il dominio B e C dominio considera attendibile il dominio B, quindi A considera attendibile il dominio del dominio C.

Esiste una differenza tra autenticazione e autorizzazione. Con l'autenticazione, il sistema dimostra che sei che è vera. Con l'autorizzazione, il sistema verifica di disporre dei diritti per eseguire l'operazione che si desidera eseguire. Per sfruttare l'analogia border al passaggio successivo, l'autenticazione viene semplicemente che il denaro necessario per è il proprietario corretto di un valido passport non necessariamente autorizzare il denaro necessario per immettere un paese. Residenti in un determinato paese sono autorizzati a immettere un altro paese, semplicemente presentare un passport solo nelle situazioni in cui il paese immesso concede l'autorizzazione illimitato per tutti i cittadini di quel determinato paese di immettere.

Analogamente, è possibile concedere tutti gli utenti da un determinate dominio le autorizzazioni per accedere a una risorsa. Qualsiasi utente che appartiene al dominio ha accesso alla risorsa, come Canada cittadini degli Stati Uniti immettiamo Canada. Tuttavia, cittadini degli Stati Uniti il tentativo di immettere Brasile o India scoprire che non potranno immettere tali paesi semplicemente presentando un passport poiché entrambi questi paesi richiedono di visitare cittadini degli Stati Uniti per avere un visa valido. Di conseguenza, l'autenticazione non garantisce l'accesso alle risorse o autorizzazione a utilizzare le risorse.

## <a name="credentials"></a>Credenziali
Passport e visti probabilmente associati sono le credenziali per un denaro necessario per accettati. Tuttavia, tali credenziali potrebbero non consentire un denaro necessario per immettere o accedere a tutte le risorse all'interno di un paese. Ad esempio, sono necessarie credenziali aggiuntive per partecipare a una conferenza. In Windows, le credenziali possono essere gestite per renderlo possibili per i titolari di account accedere alle risorse in rete senza ripetutamente fornire le credenziali. Questo tipo di accesso consente agli utenti di essere autenticati una volta dal sistema per accedere a tutte le applicazioni e origini dati che essi sono autorizzati a usare senza immettere un altro identificatore di account o la password. La piattaforma Windows in grado di sfruttare la possibilità di usare una singola identità utente (gestita da Active Directory) attraverso la rete da localmente la memorizzazione nella cache le credenziali dell'utente nell'autorità di sicurezza locale (LSA) del sistema operativo. Quando un utente accede al dominio, pacchetti di autenticazione di Windows in modo trasparente usano le credenziali per fornire accesso single sign-on per autenticare le credenziali alle risorse di rete. Per ulteriori informazioni sulle credenziali, vedere [processi di credenziali in autenticazione di Windows](credentials-processes-in-windows-authentication.md).

Un metodo di autenticazione a più fattori per il denaro necessario per potrebbe essere la necessità di eseguire e presentare più documenti per autenticare sua identità, ad esempio le informazioni di registrazione passport e conferenze. Windows implementa questo modulo o autenticazione tramite smart card, smart card virtuali e tecnologie biometriche. 

## <a name="security-principals-and-accounts"></a>Gli account e le entità di sicurezza
In Windows, qualsiasi utente, servizio, gruppo o computer in cui è possibile avviare l'azione è un'entità di sicurezza. Entità di sicurezza dispongono di account, che può essere locale a un computer o essere basati su dominio. Ad esempio, i computer appartenenti a un dominio di Windows client possono partecipare in un dominio di rete mediante la comunicazione con un controller di dominio, anche quando nessun utente umano è connesso. Per avviare le comunicazioni, il computer deve disporre di un account attivo nel dominio. Prima di accettare le comunicazioni dal computer, l'autorità di sicurezza locali sul controller di dominio autentica l'identità del computer e quindi definisce il contesto di sicurezza del computer, come nel caso di un'entità di sicurezza umano. Questo contesto di sicurezza definisce le identità e capacità di un utente o un servizio in un determinato computer o un utente, servizio, gruppo o computer in una rete. Ad esempio, definisce le risorse, ad esempio una condivisione file o stampanti, che è possibile accedere e le azioni, ad esempio lettura, scrittura o modifica, che può essere eseguita da un utente, un servizio o un computer in tale risorsa. Per ulteriori informazioni, vedere [entità di sicurezza](https://technet.microsoft.com/itpro/windows/keep-secure/security-principals).

Un account è un mezzo per identificare il richiedente, l'utente umano o il servizio, la richiesta di accesso o le risorse. Il denaro necessario per che contiene il passport autentico dispone di un account con il paese di host. Gli utenti, gruppi di utenti, oggetti e i servizi possono avere gli account individuali o condividere gli account. Gli account possono essere membri di gruppi e possono essere assegnati le autorizzazioni e diritti specifici. Account può essere limitato al computer locale, gruppo di lavoro, rete o assegnare l'appartenenza a un dominio.

Gli account predefiniti e i gruppi di sicurezza, di cui sono membri, sono definiti in ogni versione di Windows. Utilizzando i gruppi di sicurezza, è possibile assegnare le stesse autorizzazioni di sicurezza per molti utenti vengono autenticati correttamente, che semplifica l'amministrazione di accesso. Regole per il rilascio di passaporti potrebbe essere necessario che il denaro necessario per assegnare a determinati gruppi, ad esempio business, o turistica o governo. Questo processo assicura le autorizzazioni di protezione coerente tra tutti i membri di un gruppo. Utilizzando i gruppi di sicurezza per assegnare le autorizzazioni significa che l'accesso al controllo delle risorse rimane costante e facile da gestire e controllare. Aggiungendo e rimuovendo gli utenti che richiedono l'accesso da gruppi di protezione appropriati in base alle esigenze, è possibile ridurre la frequenza delle modifiche introdotte in controllo elenchi di accesso (ACL).

Gli account del servizio gestiti autonomi e gli account virtuali sono state introdotte in Windows Server 2008 R2 e Windows 7 per assicurare ad applicazioni necessarie, ad esempio Microsoft Exchange Server e Internet Information Services (IIS), l'isolamento dei relativi account di dominio, mentre eliminando manualmente la necessità di un amministratore di amministrare il nome dell'entità servizio (SPN) e le credenziali per questi account. Gli account di servizio gestito del gruppo sono stati introdotti in Windows Server 2012 e offre le stesse funzionalità all'interno del dominio ma estende tali funzionalità su più server. Quando ci si connette a un servizio ospitato in una server farm, ad esempio bilanciamento carico di rete, i protocolli di autenticazione supportano l'autenticazione reciproca richiedono che tutte le istanze dei servizi utilizzino la stessa entità.

Per ulteriori informazioni sugli account, vedere:

-   [Account di Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-accounts)

-   [Gruppi di sicurezza Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-security-groups)

-   [Account locali](https://technet.microsoft.com/itpro/windows/keep-secure/local-accounts)

-   [Account Microsoft](https://technet.microsoft.com/itpro/windows/keep-secure/microsoft-accounts)

-   [Account del servizio](https://technet.microsoft.com/itpro/windows/keep-secure/service-accounts)

-   [Identità speciali](https://technet.microsoft.com/itpro/windows/keep-secure/special-identities)

## <a name="delegated-authentication"></a>Autenticazione delegata
Per usare l'analogia viaggi, paesi potrebbero emettere lo stesso accesso a tutti i membri di una delega governativo ufficiale, a condizione che i delegati sono ben noti. Questa delega facciamo un membro agire l'autorità di un altro membro. In Windows, l'autenticazione delegata si verifica quando un servizio di rete accetta una richiesta di autenticazione da un utente e si presuppone che l'identità dell'utente per avviare una nuova connessione a un servizio di rete secondo. Per supportare l'autenticazione delegata, è necessario stabilire server front-end o di primo livello, ad esempio server web, che sono responsabili per la gestione delle richieste di autenticazione client e i server back-end o a più livelli, ad esempio un database di grandi dimensioni, che sono responsabile per l'archiviazione di informazioni. È possibile delegare il diritto di configurare l'autenticazione delegata agli utenti dell'organizzazione per ridurre il carico amministrativo gli amministratori.

Mediante la definizione di un servizio o un computer come trusted per la delega, è che il servizio o computer completare l'autenticazione delegata, ricevere un ticket per l'utente che effettua la richiesta e quindi accedere a informazioni per tale utente. Questo modello limita accesso ai dati nei server back-end solo agli utenti o ai servizi che presenti credenziali con il token del controllo di accesso corrette. Inoltre, consente di controllo dell'accesso di tali risorse back-end. Richiedendo che le credenziali delegate per il server da utilizzare per conto del client di accedere tutti i dati, assicurarsi che il server non possa essere compromessi e che è possibile accedere a informazioni riservate che viene archiviato in altri server. Autenticazione delegata è utile per le applicazioni multilivello che sono progettate per usare funzionalità single sign-on in più computer.

### <a name="authentication-in-trust-relationships-between-domains"></a>Autenticazione in relazioni di trust tra domini
La maggior parte delle organizzazioni che dispongono di più di un dominio effettivamente necessitano per gli utenti di accedere alle risorse condivise che si trovano in un dominio diverso, così come il denaro necessario per è consentito viaggio per diverse aree del paese. Controllare l'accesso richiede che gli utenti in un dominio inoltre possono essere autenticati e autorizzati a usare le risorse in un altro dominio. Per fornire funzionalità di autenticazione e autorizzazione tra client e server in domini diversi, deve esistere una relazione di trust tra i due domini. I trust sono la tecnologia sottostante mediante il quale si verificano comunicazioni protette di Active Directory e sono un componente integrante di sicurezza dell'architettura di rete di Windows Server.

Quando esiste un trust tra due domini, i meccanismi di autenticazione per ogni dominio attendibile le autenticazioni provenienti da altro dominio. Trust forniscono per controllo dell'accesso a risorse condivise in un dominio di risorse, il dominio trusting, verificando che l'autenticazione in ingresso le richieste provengano da un'autorità attendibile, il dominio trusted. In questo modo, i trust agiscono come bridge che consentono solo convalidata viaggio le richieste di autenticazione tra domini.

Come una relazione di trust specifico passa le richieste di autenticazione varia a seconda della configurazione. Relazioni di trust possono essere bidirezionale o unidirezionale, fornendo accesso dal dominio trusted alle risorse nel dominio trusting, fornendo accesso di ciascun dominio alle risorse in altro dominio. Considera attendibili anche sono entrambi non transitivo, in cui trust maiuscole o è presente solo tra i domini di partner, due trust transitivo, nel qual caso trust estende automaticamente a qualsiasi altro domini trusting entrambi i partner.

Per informazioni sul funzionamento di una relazione di trust, vedere [come dominio e foresta trust lavoro](https://technet.microsoft.com/library/cc773178(v=ws.10).aspx).

### <a name="protocol-transition"></a>Transizione al protocollo
Transizione al protocollo assiste gli sviluppatori di applicazioni, consentendo alle applicazioni supportano i meccanismi di autenticazione al livello di autenticazione utente e passando al protocollo Kerberos per le funzionalità di sicurezza, ad esempio l'autenticazione reciproca e delega vincolata, in livelli di successiva dell'applicazione.

Per ulteriori informazioni sulla transizione al protocollo, vedere [transizione al protocollo Kerberos e delega vincolata](https://technet.microsoft.com/library/cc758097(v=ws.10).aspx).

### <a name="constrained-delegation"></a>Delega vincolata
La delega vincolata offre agli amministratori la possibilità di specificare e applicare limiti di trust per applicazioni, limitando l'ambito in cui servizi delle applicazioni possono agire per conto di un utente. È possibile specificare determinati servizi da cui un computer non trusted per delega può richiedere risorse. La flessibilità necessaria per limitare i diritti di autorizzazione per i servizi consente di migliorare la progettazione di protezione dell'applicazione, riducendo le opportunità per la compromissione da servizi non attendibili.

Per ulteriori informazioni sulla delega vincolata, vedere [Panoramica della delega vincolata Kerberos](../kerberos/kerberos-constrained-delegation-overview.md).

## <a name="see-also"></a>Vedere anche
[Panoramica tecnica dell'accesso a Windows e autenticazione](https://technet.microsoft.com/library/dn269029.aspx)


