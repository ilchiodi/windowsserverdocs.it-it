---
title: Concetti su Autenticazione di Windows
description: Sicurezza di Windows Server
ms.prod: windows-server
ms.technology: security-windows-auth
ms.topic: article
ms.assetid: 29d1db15-cae0-4e3d-9d8e-241ac206bb8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 4051bfea26d5c96d02132b50373f56b7b17ce5fb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857474"
---
# <a name="windows-authentication-concepts"></a>Concetti su Autenticazione di Windows

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questa panoramica di riferimento vengono descritti i concetti su cui si basa l'autenticazione di Windows.

L'autenticazione è un processo che consente di verificare l'identità di un oggetto o di una persona. Quando si autentica un oggetto, lo scopo è verificare che tale oggetto sia autentico. Quando si autentica una persona, l'obiettivo consiste nel verificare che la persona non sia un impostore.

In un contesto di rete, l'autorizzazione consiste nel dimostrare l'identità a un'applicazione o risorsa di rete. In genere, l'identità viene dimostrata da un'operazione di crittografia che utilizza una chiave nota solo all'utente (come la crittografia a chiave pubblica) o una chiave condivisa. Il lato server dello scambio di autenticazione confronta i dati firmati con una chiave crittografica nota per convalidare il tentativo di autenticazione.

L'archiviazione delle chiavi crittografiche in una posizione centralizzata sicura rende l'autenticazione un processo scalabile e gestibile. Active Directory è la tecnologia consigliata e predefinita per l'archiviazione delle informazioni di identità, incluse le chiavi crittografiche che rappresentano le credenziali dell'utente. Active Directory è necessario per le implementazioni NTLM e Kerberos predefinite.

Le tecniche di autenticazione variano da un semplice accesso a un sistema operativo o da un accesso a un servizio o a un'applicazione, che identifica gli utenti in base a un elemento che solo l'utente conosce, ad esempio una password, a meccanismi di sicurezza più potenti che usano un elemento che l'utente has'such come token, certificati di chiave pubblica, immagini o attributi biologici. In un ambiente aziendale, gli utenti possono accedere a più applicazioni su numerosi tipi di server nell'ambito di un solo percorso o su più percorsi. Per tali motivi, l'autenticazione deve supportare ambienti per altre piattaforme e per altri sistemi operativi Windows.

## <a name="authentication-and-authorization-a-travel-analogy"></a>Autenticazione e autorizzazione: un'analogia di viaggio
Un'analogia di viaggio può essere utile per spiegare il funzionamento dell'autenticazione. Alcune attività preparatorie sono in genere necessarie per iniziare il viaggio. Il viaggiatore deve dimostrare la propria identità reale alle autorità host. Questa prova può essere sotto forma di prova di cittadinanza, luogo di nascita, voucher personale, fotografie o qualsiasi altra richiesta da parte della legge del paese host. L'identità del viaggiatore viene convalidata dal rilascio di un passaporto, che è analogo a un account di sistema rilasciato e amministrato da un'organizzazione, ovvero l'entità di sicurezza. Il passaporto e la destinazione prevista sono basati su un set di regole e normative emesse dall'autorità governativa.

**Il viaggio**

Quando il viaggiatore arriva al confine internazionale, una guardia dei confini chiede le credenziali e il viaggiatore presenta il proprio passaporto. Il processo è due volte:

-   Il servizio Guard autentica il passaporto verificando che sia stato emesso da un'autorità di sicurezza che il governo locale considera attendibile, almeno per emettere passaporti, e verificando che il passaporto non sia stato modificato.

-   Il Guard autentica il viaggiatore verificando che la faccia corrisponda a quella della persona raffigurata sul passaporto e che le altre credenziali necessarie siano in ordine corretto.

Se il passaporto risulta essere valido e il viaggiatore dimostra di essere il proprietario, l'autenticazione ha esito positivo e il viaggiatore può essere autorizzato ad accedere attraverso il bordo.

Il trust transitivo tra le autorità di sicurezza è la base dell'autenticazione; il tipo di autenticazione che si verifica in un bordo internazionale è basato sulla relazione di trust. Il governo locale non conosce il viaggiatore, ma considera attendibile il governo dell'host. Quando il governo dell'host ha emesso il passaporto, non ha conosciuto il viaggiatore. Considerato attendibile dall'agenzia che ha emesso il certificato di nascita o altra documentazione. L'agenzia che ha emesso il certificato di nascita, a sua volta, è considerata attendibile dal medico che ha firmato il certificato. Il medico ha testimoniato la nascita del viaggiatore e ha timbrato il certificato con una prova diretta dell'identità, in questo caso con il footprint del neonato. Il trust trasferito in questo modo, tramite intermediari attendibili, è transitivo.

Il trust transitivo è la base per la sicurezza di rete nell'architettura client/server di Windows. Una relazione di trust scorre in un set di domini, ad esempio un albero di dominio, e forma una relazione tra un dominio e tutti i domini che considerano attendibile tale dominio. Ad esempio, se il dominio A ha una relazione di trust transitiva con il dominio B e se il dominio B considera attendibile il dominio C, il dominio A considera attendibile il dominio C.

Esiste una differenza tra l'autenticazione e l'autorizzazione. Con l'autenticazione, il sistema dimostra che l'utente è quello che si sta affermando. Con l'autorizzazione, il sistema verifica che l'utente disponga dei diritti necessari per eseguire le operazioni desiderate. Per eseguire l'analogia del bordo al passaggio successivo, semplicemente l'autenticazione che il viaggiatore è il proprietario appropriato di un passaporto valido non autorizza necessariamente il viaggiatore a entrare in un paese. I residenti di un determinato paese possono entrare in un altro paese semplicemente presentando un passaporto solo nelle situazioni in cui il paese immesso concede l'autorizzazione illimitata a tutti i cittadini di quel paese specifico.

Analogamente, è possibile concedere a tutti gli utenti di un determinato dominio le autorizzazioni per accedere a una risorsa. Tutti gli utenti che appartengono a tale dominio hanno accesso alla risorsa, così come il Canada consente ai cittadini statunitensi di entrare in Canada. Tuttavia, i cittadini degli Stati Uniti che tentano di entrare in Brasile o India scoprono che non possono entrare in questi paesi semplicemente presentando un passaporto perché entrambi questi paesi richiedono che i cittadini degli Stati Uniti abbiano un visto valido. Pertanto, l'autenticazione non garantisce l'accesso alle risorse o all'autorizzazione per l'uso delle risorse.

## <a name="credentials"></a>Credenziali
Un passaporto e i visti possibilmente associati sono le credenziali accettate per un viaggiatore. Tuttavia, tali credenziali potrebbero non consentire a un viaggiatore di accedere a tutte le risorse all'interno di un paese. Ad esempio, sono necessarie credenziali aggiuntive per partecipare a una conferenza. In Windows le credenziali possono essere gestite per consentire ai titolari dell'account di accedere alle risorse in rete senza dover specificare ripetutamente le proprie credenziali. Questo tipo di accesso consente agli utenti di essere autenticati una sola volta dal sistema per accedere a tutte le applicazioni e alle origini dati che sono autorizzate a usare senza immettere un altro identificatore o password dell'account. La piattaforma Windows sfrutta la possibilità di usare una singola identità utente (gestita da Active Directory) attraverso la rete memorizzando localmente nella cache le credenziali utente nell'autorità di sicurezza locale (LSA) del sistema operativo. Quando un utente accede al dominio, i pacchetti di autenticazione di Windows usano in modo trasparente le credenziali per fornire Single Sign-On durante l'autenticazione delle credenziali per le risorse di rete. Per ulteriori informazioni sulle credenziali, vedere [processi di credenziali nell'autenticazione di Windows](credentials-processes-in-windows-authentication.md).

Una forma di multi-factor authentication per il viaggiatore potrebbe essere la necessità di portare e presentare più documenti per autenticare la propria identità, ad esempio le informazioni di registrazione di un passaporto e una conferenza. Windows implementa questo modulo o l'autenticazione tramite smart card, smart card virtuali e tecnologie biometriche. 

## <a name="security-principals-and-accounts"></a>Entità di sicurezza e account
In Windows qualsiasi utente, servizio, gruppo o computer che può avviare un'azione è un'entità di sicurezza. Le entità di sicurezza hanno account, che possono essere locali in un computer o essere basati su dominio. Ad esempio, i computer Windows client aggiunti a un dominio possono partecipare a un dominio di rete comunicando con un controller di dominio anche quando nessun utente umano è connesso. Per avviare le comunicazioni, il computer deve disporre di un account attivo nel dominio. Prima di accettare le comunicazioni dal computer, l'autorità di sicurezza locale nel controller di dominio autentica l'identità del computer e quindi definisce il contesto di sicurezza del computer esattamente come per un'entità di protezione umana. Questo contesto di sicurezza definisce l'identità e le funzionalità di un utente o di un servizio su un computer o un utente, un servizio, un gruppo o un computer specifico in una rete. Ad esempio, definisce le risorse, ad esempio una condivisione file o una stampante, a cui è possibile accedere e le azioni, ad esempio la lettura, la scrittura o la modifica, che possono essere eseguite da un utente, un servizio o un computer su tale risorsa. Per altre informazioni, vedere [entità di sicurezza](https://technet.microsoft.com/itpro/windows/keep-secure/security-principals).

Un account è un mezzo per identificare un richiedente, ovvero l'utente o il servizio umano, che richiede l'accesso o le risorse. Il viaggiatore che possiede il passaporto autentico possiede un account con il paese host. Utenti, gruppi di utenti, oggetti e servizi possono avere tutti gli account singoli o di condivisione. Gli account possono essere membri di gruppi e possono essere assegnati a diritti e autorizzazioni specifici. Gli account possono essere limitati al computer locale, al gruppo di lavoro, alla rete o all'appartenenza a un dominio.

Gli account predefiniti e i gruppi di sicurezza di cui sono membri vengono definiti in ogni versione di Windows. Utilizzando i gruppi di sicurezza, è possibile assegnare le stesse autorizzazioni di sicurezza a molti utenti che sono stati autenticati correttamente, semplificando l'amministrazione dell'accesso. Le regole per il rilascio dei passaporti possono richiedere che il viaggiatore venga assegnato a determinati gruppi, ad esempio business, o Tourist o Government. Questo processo garantisce le autorizzazioni di sicurezza coerenti in tutti i membri di un gruppo. L'uso dei gruppi di sicurezza per assegnare le autorizzazioni significa che il controllo di accesso delle risorse rimane costante e facile da gestire e controllare. Con l'aggiunta e la rimozione di utenti che richiedono l'accesso dai gruppi di sicurezza appropriati in base alle esigenze, è possibile ridurre al minimo la frequenza delle modifiche apportate agli elenchi di controllo di accesso (ACL).

Gli account dei servizi gestiti autonomi e gli account virtuali sono stati introdotti in Windows Server 2008 R2 e Windows 7 per fornire le applicazioni necessarie, ad esempio Microsoft Exchange Server e Internet Information Services (IIS), con l'isolamento dei propri account di dominio, eliminando la necessità di un amministratore di amministrare manualmente il nome dell'entità servizio (SPN) e le credenziali per questi account. Gli account del servizio gestiti del gruppo sono stati introdotti in Windows Server 2012 e forniscono le stesse funzionalità all'interno del dominio, ma anche la funzionalità viene estesa su più server. Durante la connessione a un servizio ospitato in una server farm, ad esempio Bilanciamento carico di rete, i protocolli di autenticazione che supportano l'autenticazione reciproca richiedono che tutte le istanze dei servizi utilizzino la stessa entità.

Per ulteriori informazioni sugli account, vedere:

-   [Account Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-accounts)

-   [Gruppi di sicurezza Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-security-groups)

-   [Account locali](https://technet.microsoft.com/itpro/windows/keep-bastion.local-accounts)

-   [Account Microsoft](https://technet.microsoft.com/itpro/windows/keep-secure/microsoft-accounts)

-   [Account del servizio](https://technet.microsoft.com/itpro/windows/keep-secure/service-accounts)

-   [Identità speciali](https://technet.microsoft.com/itpro/windows/keep-secure/special-identities)

## <a name="delegated-authentication"></a>Autenticazione delegata
Per usare l'analogia di viaggio, i paesi potrebbero emettere lo stesso accesso a tutti i membri di una delega governativa ufficiale, purché i delegati siano noti. Questa delega consente a un membro di agire sull'autorità di un altro membro. In Windows, l'autenticazione delegata si verifica quando un servizio di rete accetta una richiesta di autenticazione da un utente e presuppone l'identità di tale utente per avviare una nuova connessione a un secondo servizio di rete. Per supportare l'autenticazione delegata, è necessario stabilire server front-end o di primo livello, ad esempio server Web, responsabili della gestione delle richieste di autenticazione client e dei server back-end o a più livelli, ad esempio database di grandi dimensioni, responsabili dell'archiviazione delle informazioni. È possibile delegare il diritto di configurare l'autenticazione delegata agli utenti dell'organizzazione per ridurre il carico amministrativo degli amministratori.

Stabilendo un servizio o un computer considerato attendibile per la delega, si consente al servizio o al computer di completare l'autenticazione delegata, ricevere un ticket per l'utente che effettua la richiesta e quindi accedere alle informazioni per tale utente. Questo modello limita l'accesso ai dati nei server back-end solo agli utenti o ai servizi che presentano le credenziali con i token di controllo di accesso corretti. Consente inoltre di controllare l'accesso a tali risorse di back-end. Richiedendo che tutti i dati siano accessibili per mezzo di credenziali delegate al server per l'utilizzo da parte del client, è necessario assicurarsi che il server non possa essere compromesso e che sia possibile accedere a informazioni riservate archiviate in altri server. L'autenticazione delegata è utile per le applicazioni multilivello progettate per utilizzare Single Sign-On funzionalità in più computer.

### <a name="authentication-in-trust-relationships-between-domains"></a>Autenticazione in relazioni di trust tra domini
Per la maggior parte delle organizzazioni che dispongono di più di un dominio è necessario che gli utenti accedano a risorse condivise che si trovano in un dominio diverso, proprio come il viaggiatore è autorizzato a spostarsi in aree diverse del paese. Per controllare questo accesso è necessario che gli utenti di un dominio possano essere autenticati e autorizzati a usare le risorse in un altro dominio. Per fornire funzionalità di autenticazione e autorizzazione tra client e server in domini diversi, è necessario che esista una relazione di trust tra i due domini. I trust sono la tecnologia sottostante attraverso cui si verificano le comunicazioni Active Directory protette e costituiscono un componente di sicurezza integrale dell'architettura di rete di Windows Server.

Quando esiste una relazione di trust tra due domini, i meccanismi di autenticazione per ogni dominio considerano attendibili le autenticazioni provenienti dall'altro dominio. I trust contribuiscono a garantire l'accesso controllato alle risorse condivise in un dominio di risorse, ovvero il dominio trusting, verificando che le richieste di autenticazione in ingresso provengano da un'autorità attendibile, ovvero il dominio trusted. In questo modo, i trust fungono da Bridge che consentono solo la convalida delle richieste di autenticazione tra domini.

Il modo in cui un trust specifico passa le richieste di autenticazione dipende dalla configurazione. Le relazioni di trust possono essere unidirezionali, fornendo l'accesso dal dominio trusted alle risorse nel dominio trusting, oppure bidirezionale, fornendo l'accesso da ogni dominio alle risorse nell'altro dominio. I trust sono anche non transitivi, nel qual caso il trust esiste solo tra i due domini trust partner, o transitivi, nel qual caso l'attendibilità si estende automaticamente a qualsiasi altro dominio considerato attendibile da uno dei partner.

Per informazioni sul funzionamento di un trust, vedere funzionamento dei [trust tra domini e foreste](https://technet.microsoft.com/library/cc773178(v=ws.10).aspx).

### <a name="protocol-transition"></a>Transizione del protocollo
La transizione del protocollo consente ai progettisti di applicazioni di supportare diversi meccanismi di autenticazione a livello di autenticazione utente e di passare al protocollo Kerberos per le funzionalità di sicurezza, ad esempio l'autenticazione reciproca e la delega vincolata, nei livelli applicazione successivi.

Per ulteriori informazioni sulla transizione del protocollo, vedere la pagina relativa alla [transizione del protocollo Kerberos e alla delega vincolata](https://technet.microsoft.com/library/cc758097(v=ws.10).aspx).

### <a name="constrained-delegation"></a>Delega vincolata
La delega vincolata offre agli amministratori la possibilità di specificare e applicare limiti di trust per le applicazioni, limitando l'ambito in cui i servizi applicativi possono agire per conto di un utente. È possibile specificare determinati servizi da cui un computer considerato attendibile per la delega può richiedere risorse. La flessibilità di vincolare i diritti di autorizzazione per i servizi consente di migliorare la progettazione della sicurezza delle applicazioni riducendo le opportunità di compromissione da parte di servizi non attendibili.

Per ulteriori informazioni sulla delega vincolata, vedere [Cenni preliminari sulla delega vincolata Kerberos](../kerberos/kerberos-constrained-delegation-overview.md).

## <a name="see-also"></a>Vedere anche
[Panoramica tecnica dell'accesso e dell'autenticazione di Windows](https://technet.microsoft.com/library/dn269029.aspx)


