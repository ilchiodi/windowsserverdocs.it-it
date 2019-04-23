---
title: Concetti su Autenticazione di Windows
description: Sicurezza di Windows Server
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
ms.openlocfilehash: ee3fa19efa8c5098008749a467e649edcb5bb716
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876502"
---
# <a name="windows-authentication-concepts"></a>Concetti su Autenticazione di Windows

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento di panoramica di riferimento descrive i concetti su cui si basa l'autenticazione di Windows.

L'autenticazione è un processo che consente di verificare l'identità di un oggetto o di una persona. Quando si autentica un oggetto, lo scopo è verificare che tale oggetto sia autentico. Quando si autentica una persona, l'obiettivo consiste nel verificare che la persona non sia un impostore.

In un contesto di rete, l'autorizzazione consiste nel dimostrare l'identità a un'applicazione o risorsa di rete. In genere, l'identità viene dimostrata tramite un'operazione di crittografia che usa una chiave solo l'utente sa (come con crittografia a chiave pubblica) o una chiave condivisa. Il lato server dello scambio di autenticazione confronta i dati firmati con una chiave crittografica nota per convalidare il tentativo di autenticazione.

L'archiviazione delle chiavi crittografiche in una posizione centralizzata sicura rende l'autenticazione un processo scalabile e gestibile. Active Directory è consigliata e tasti di tecnologia predefinita per l'archiviazione delle informazioni di identità, che includono il servizio di crittografia sono le credenziali dell'utente. Active Directory è necessario per le implementazioni NTLM e Kerberos predefinite.

Le tecniche di autenticazione variare da un accesso semplice a un sistema operativo o di un accesso a un servizio o applicazione, che identifica gli utenti in base a un elemento che è nota solo all'utente, ad esempio una password, ai meccanismi di sicurezza più potenti che utilizzano informazioni che has'such utente come i token, certificati di chiave pubblica, immagini o attributi biologici. In un ambiente aziendale, gli utenti possono accedere a più applicazioni su numerosi tipi di server nell'ambito di un solo percorso o su più percorsi. Per tali motivi, l'autenticazione deve supportare ambienti per altre piattaforme e per altri sistemi operativi Windows.

## <a name="authentication-and-authorization-a-travel-analogy"></a>Autenticazione e autorizzazione: Un'analogia travel
Un'analogia travel consentono di spiegare come funziona l'autenticazione. Alcune attività preliminari vengono in genere necessari per iniziare il viaggio. Il viaggiatore deve dimostrare la propria identità true per le autorità di host. Questo modello può essere sotto forma di verifica della cittadinanza, luogo di nascita, un voucher personali, fotografie o qualsiasi elemento è richiesto dalla legge del paese host. Identità di bagagli viene convalidata per l'emissione di un account passport, che è analogo a un account di sistema emesso e amministrati da un'organizzazione, l'entità di sicurezza. Passport e la destinazione desiderata si basano su un set di regole e norme rilasciati dall'autorità governative.

**Il percorso**

Quando arriva il denaro necessario per il bordo internazionale, una clausola guard bordo richiede le credenziali e il denaro necessario per presenta la propria passport. Il processo prevede due fasi:

-   La clausola guard autentica il passport verificando che sia stato emesso da un'autorità di sicurezza che il governo locale trust (trust, o almeno per il rilascio di passaporto) e verificando che il passport non è stato modificato.

-   La clausola guard per eseguire l'autenticazione il denaro necessario per la verifica che l'immagine corrisponda il viso della persona nella figura di passport e che altre credenziali necessari siano in buono stato.

Se il passport è senza dubbio valido e il viaggiatore è senza dubbio relativo proprietario, l'autenticazione ha esito positivo e il denaro necessario per può essere consentito l'accesso tra il bordo.

Trust transitivo tra le autorità di sicurezza è alla base di autenticazione. il tipo di autenticazione che ha luogo presso un bordo internazionale si basa sull'attendibilità. Non conosce il denaro necessario per il governo locale, ma considera attendibile che esegua il governo degli host. Quando l'host patenti di passport, non conoscesse il denaro necessario per essere. Considerato attendibile l'agenzia che ha emesso il certificato di nascita o altri documenti. L'agenzia che ha emesso il certificato di nascita, a sua volta, attendibili medico che ha firmato il certificato. Il medico verifica nascita i bagagli e contrassegnato in questo caso il certificato con diretta come prova dell'identità, con footprint di per. Relazione di trust che viene trasferito in questo modo, tramite intermediari attendibili, è transitiva.

Trust transitivo è la base per la sicurezza di rete nell'architettura client/server Windows. Una relazione di trust attraversa un set di domini, ad esempio un albero di dominio e costituisce una relazione tra un dominio e tutti i domini trusting del dominio. Ad esempio, se il dominio dispone di un trust transitivo con il dominio B e se il dominio B considera attendibile il dominio C, quindi il dominio considera attendibile il dominio C.

C'è una differenza tra l'autenticazione e autorizzazione. Con l'autenticazione, il sistema si riveli identità dell'utente. Con l'autorizzazione, il sistema di verifica di disporre delle autorizzazioni per eseguire l'operazione che si desidera eseguire. Per sfruttare la stessa analogia del bordo al passaggio successivo, si limita l'autenticazione che il denaro necessario per sia il proprietario appropriato di un valido passport non autorizza necessariamente il denaro necessario per l'immissione di un paese. Immettere un altro paese, semplicemente presentando un passaporto solo in situazioni in cui il paese immesso concede l'autorizzazione illimitata per tutti i cittadini di quel particolare paese immettere possono residenti in un paese specifico.

Analogamente, è possibile concedere tutti gli utenti da un determinate dominio le autorizzazioni per accedere a una risorsa. Qualsiasi utente che appartiene al dominio ha accesso alla risorsa, esattamente come Canada consente anche i cittadini statunitensi immettere Canada. Tuttavia, anche i cittadini statunitensi tenta di immettere Brasile o India disponibili che non potranno immettere i paesi in questione si limita presentando un passaporto perché i paesi in questione non richiedono visita anche i cittadini statunitensi per avere visto valido. Di conseguenza, l'autenticazione non garantisce l'accesso alle risorse o di autorizzazione per l'uso delle risorse.

## <a name="credentials"></a>Credenziali
Passport e visti eventualmente associati sono le credenziali accettate per una viaggiatore. Tuttavia, tali credenziali potrebbero non consentire un denaro necessario per immettere o accedere a tutte le risorse all'interno di un paese. Ad esempio, sono necessarie credenziali aggiuntive di partecipare a una conferenza. In Windows, le credenziali possono essere gestite da rendere possibile la titolari dell'account accedere alle risorse in rete senza ripetutamente fornire le proprie credenziali. Questo tipo di accesso consente agli utenti di essere autenticato una sola volta dal sistema per accedere a tutte le applicazioni e le origini dati che sono autorizzati a utilizzare senza immettere un altro identificatore di account o password. La piattaforma Windows sfrutta la possibilità di utilizzare una singola identità utente (gestita da Active Directory) attraverso la rete memorizzando localmente nella cache le credenziali dell'utente in Autorità di protezione locale (LSA) del sistema operativo. Quando un utente accede al dominio, i pacchetti di autenticazione di Windows usano in modo trasparente le credenziali per fornire accesso single sign-on per autenticare le credenziali per le risorse di rete. Per altre informazioni sulle credenziali, vedere [processi di credenziali di autenticazione basata su Windows](credentials-processes-in-windows-authentication.md).

Una forma di multi-factor authentication per il viaggiatore potrebbe essere la necessità di eseguire e presentare più documenti per l'autenticazione propria identità, ad esempio le informazioni di registrazione passport e di conferenza. Windows implementa questo form o l'autenticazione tramite smart card, le smart card virtuali e le tecnologie biometriche. 

## <a name="security-principals-and-accounts"></a>Gli account e le entità di sicurezza
In Windows, qualsiasi utente, servizio, gruppo o computer in cui è possibile avviare l'azione è un'entità di sicurezza. Le entità di sicurezza dispongono di account, che può essere locale a un computer o basato su dominio. Ad esempio, i computer aggiunti a un dominio di Windows client possono partecipare in un dominio di rete mediante la comunicazione con un controller di dominio anche quando non è connesso alcun utente umano. Per avviare le comunicazioni, il computer deve avere un account attivo del dominio. Prima di accettare le comunicazioni dal computer, l'autorità di sicurezza locale nel controller di dominio autentica l'identità del computer e quindi definisce il contesto di sicurezza del computer esattamente come verrebbe per un'entità di sicurezza risorse umane. In questo contesto di sicurezza definisce le identità e le funzionalità di un utente o un servizio in un computer specifico o un utente, servizio, gruppo o computer in una rete. Ad esempio, definisce le risorse, ad esempio una condivisione file o stampanti, che è possibile accedere e le azioni, ad esempio lettura, scrittura o modifica, che può essere eseguita da un utente, un servizio o un computer su tale risorsa. Per altre informazioni, vedere [le entità di sicurezza](https://technet.microsoft.com/itpro/windows/keep-secure/security-principals).

Un account è un mezzo per identificare il richiedente, l'utente umano o il servizio, che richiede l'accesso o le risorse. Viaggiatore che detiene il passport autentico possiede un account con il paese ospite. Gli utenti, gruppi di utenti, gli oggetti e i servizi possono avere account singoli o condividere gli account. Gli account possono essere membro dei gruppi e possono essere assegnati le autorizzazioni e diritti specifici. Gli account possono essere limitati per il computer locale, un gruppo di lavoro, una rete oppure essere assegnati l'appartenenza a un dominio.

Gli account predefiniti e i gruppi di sicurezza, di cui sono membri, sono definiti in ogni versione di Windows. Usando i gruppi di sicurezza, è possibile assegnare le stesse autorizzazioni di sicurezza per molti utenti che saranno stati autenticati, che semplifica l'amministrazione dell'accesso. Le regole per il rilascio di passaporto potrebbero richiedere che il denaro necessario per essere assegnato a determinati gruppi, ad esempio azienda, turistico o per enti pubblici. Questo processo assicura le autorizzazioni di sicurezza coerente tra tutti i membri di un gruppo. Usando i gruppi di sicurezza per assegnare autorizzazioni implica che l'accesso di controllo delle risorse rimane costante e facile da gestire e controllare. Aggiungendo e rimuovendo gli utenti che richiedono l'accesso dai gruppi di sicurezza appropriati in base alle esigenze, è possibile ridurre al minimo la frequenza delle modifiche agli elenchi di controllo (ACL) di accesso.

Gli account del servizio gestiti autonomi e gli account virtuali sono state introdotte in Windows Server 2008 R2 e Windows 7 per offrire ad applicazioni necessarie, ad esempio Microsoft Exchange Server e Internet Information Services (IIS), l'isolamento del proprio dominio account, eliminando la necessità di un amministratore amministri manualmente il nome dell'entità servizio (SPN) e le credenziali per questi account. Gli account di servizio gestito del gruppo sono stati introdotti in Windows Server 2012 e offre le stesse funzionalità all'interno del dominio ma estende tali funzionalità su più server. Durante la connessione a un servizio ospitato in una server farm, ad esempio Bilanciamento carico di rete, i protocolli di autenticazione che supportano l'autenticazione reciproca richiedono che tutte le istanze dei servizi utilizzino la stessa entità.

Per altre informazioni sugli account, vedere:

-   [Account di Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-accounts)

-   [Gruppi di sicurezza di Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-security-groups)

-   [Account locali](https://technet.microsoft.com/itpro/windows/keep-bastion.local-accounts)

-   [Account Microsoft](https://technet.microsoft.com/itpro/windows/keep-secure/microsoft-accounts)

-   [Account del servizio](https://technet.microsoft.com/itpro/windows/keep-secure/service-accounts)

-   [Identità speciali](https://technet.microsoft.com/itpro/windows/keep-secure/special-identities)

## <a name="delegated-authentication"></a>Autenticazione delegata
Per usare la stessa analogia del viaggio, paesi potrebbero eseguire lo stesso accesso a tutti i membri di una delega governativo ufficiale, purché i delegati sono noti. Questa delega consente a un membro di agire sull'autorità di un altro membro. In Windows, autenticazione delegata si verifica quando un servizio di rete accetta una richiesta di autenticazione da un utente e assume l'identità dell'utente per poter avviare una nuova connessione a un secondo servizio di rete. Per supportare l'autenticazione delegata, è necessario stabilire i server front-end o di primo livello, ad esempio server web, che sono responsabili della gestione delle richieste di autenticazione client e server back-end o a più livelli, ad esempio un database di grandi dimensioni, che sono responsabile l'archiviazione di informazioni. È possibile delegare il diritto di configurazione dell'autenticazione delegata per gli utenti dell'organizzazione per ridurre il carico amministrativo negli amministratori.

Mediante la definizione di un servizio o un computer come attendibile per la delega, è consentire a tale servizio o computer completare l'autenticazione delegata, un ticket per l'utente che effettua la richiesta di ricezione e quindi accedere alle informazioni per quell'utente. Questo modello limita l'accesso ai dati nei server back-end solo a tali utenti o servizi che presentare le credenziali con il token di controllo di accesso corretti. Inoltre, consente il controllo di accesso di tali risorse back-end. Richiedendo che tutti i dati accessibile tramite le credenziali delegate per il server per l'uso per conto del client, assicurarsi che il server non può essere compromesso e che è possibile ottenere l'accesso a informazioni riservate che vengono archiviati in altri server. Autenticazione delegata è utile per applicazioni a più livelli che sono progettate per usare funzionalità single sign-on tra più computer.

### <a name="authentication-in-trust-relationships-between-domains"></a>Autenticazione in relazioni di trust tra domini
La maggior parte delle organizzazioni che hanno più di un dominio hanno la legittima necessitano agli utenti di accedere alle risorse condivise che si trovano in un dominio diverso, proprio come se il viaggiatore è viaggio in aree diverse del paese. Il controllo di questo accesso richiede che gli utenti in un dominio anche possono essere autenticati e autorizzati a usare le risorse in un altro dominio. Per fornire funzionalità di autenticazione e autorizzazione tra client e server in domini diversi, deve esistere una relazione di trust tra i due domini. I trust sono la tecnologia sottostante mediante il quale si verificano protette le comunicazioni di Active Directory e sono un componente integrale di sicurezza dell'architettura di rete di Windows Server.

Quando esiste una relazione di trust tra due domini, i meccanismi di autenticazione per ogni dominio di trust le autenticazioni provenienti da altro dominio. I trust forniscono per l'accesso controllato alle risorse condivise nel dominio delle risorse, il dominio trusting, verificando che l'autenticazione in ingresso le richieste provengano da un'autorità attendibile, il dominio trusted. In questo modo, i trust agiscono come bridge che consentano solo convalidata viaggio di richieste di autenticazione tra domini.

La modalità con cui una relazione di trust specifico passa le richieste di autenticazione dipende dal modo in cui è configurato. Relazioni di trust possono essere unidirezionale, consentendo l'accesso dal dominio trusted alle risorse nel dominio trusting o bidirezionale, fornendo l'accesso da ogni dominio alle risorse in altro dominio. Considera attendibili anche sono entrambi non transitivo, in cui si trova case trust solo tra i domini di partner due trust, o transitivo, nel qual caso trust esteso automaticamente a tutti gli altri domini trusting di entrambi i partner.

Per informazioni sull'uso di una relazione di trust, vedere [come dominio e foresta trust lavoro](https://technet.microsoft.com/library/cc773178(v=ws.10).aspx).

### <a name="protocol-transition"></a>Transizione di protocollo
Transizione al protocollo assiste i progettisti di applicazioni, consentendo alle applicazioni di supportare i meccanismi di autenticazione diversi al livello di autenticazione utente e passando al protocollo Kerberos per le funzionalità di sicurezza, ad esempio l'autenticazione reciproca e delega vincolata, nei livelli applicazione successiva.

Per altre informazioni sulla transizione di protocollo, vedere [Kerberos Protocol Transition and Constrained Delegation](https://technet.microsoft.com/library/cc758097(v=ws.10).aspx).

### <a name="constrained-delegation"></a>Delega vincolata
La delega vincolata offre agli amministratori la possibilità di specificare e applicare limiti di attendibilità dell'applicazione, limitando l'ambito in cui servizi delle applicazioni possono agire per conto di un utente. È possibile specificare determinati servizi da cui un computer che sia trusted per delega può richiedere le risorse. La flessibilità necessaria per vincolare i diritti di autorizzazione per i servizi consente di migliorare la progettazione della sicurezza dell'applicazione riducendo le opportunità di compromissione da servizi non attendibili.

Per altre informazioni sulla delega vincolata, vedere [Panoramica della delega vincolata Kerberos](../kerberos/kerberos-constrained-delegation-overview.md).

## <a name="see-also"></a>Vedere anche
[Panoramica tecnica dell'accesso di Windows e autenticazione](https://technet.microsoft.com/library/dn269029.aspx)


