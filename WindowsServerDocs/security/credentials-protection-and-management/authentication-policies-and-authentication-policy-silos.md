---
title: Criteri di autenticazione e silo di criteri di autenticazione
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7eb0e640-033d-49b5-ab44-3959395ad567
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 55dd38c966fea8448231b06cde9c1dd63b024197
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266764"
---
# <a name="authentication-policies-and-authentication-policy-silos"></a>Criteri di autenticazione e silo di criteri di autenticazione

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento per professionisti IT sono descritti i silo di criteri di autenticazione e i criteri che possono limitare gli account in tali silo. È illustrato anche il modo in cui è possibile usare i criteri di autenticazione per limitare l'ambito degli account.

I silo di criteri di autenticazione e i criteri corrispondenti offrono un modo per includere credenziali con privilegi elevati per sistemi pertinenti solo per utenti, computer o servizi selezionati. Silo possono essere definiti e gestiti in servizi di dominio Active Directory (AD DS) tramite il centro di amministrazione di Active Directory e i cmdlet di Windows PowerShell per Active Directory.

I silo di criteri di autenticazione sono contenitori a cui gli amministratori possono assegnare account utente, account computer e account del servizio. Gli insiemi di account possono essere quindi gestiti tramite i criteri di autenticazione applicati al contenitore specifico. Ciò riduce la necessità di verifica dell'accesso alle risorse da parte dell'amministratore per i singoli account e aiuta a impedire agli utenti malintenzionati di accedere ad altre risorse tramite il furto di credenziali.

Le funzionalità introdotte in Windows Server 2012 R2, consentono di creare silo di criteri, che ospitano un insieme di utenti con privilegi elevati di autenticazione. È quindi possibile assegnare criteri di autenticazione per questo contenitore, in modo da limitare la posizione in cui gli account con privilegi possono essere usati nel dominio. Quando gli account si trovano nel gruppo di sicurezza Utenti protetti, sono applicati controlli aggiuntivi, ad esempio l'uso esclusivo del protocollo Kerberos.

Queste capacità permettono di limitare l'uso di account di valore elevato a host di valore elevato. Ad esempio, è possibile creare un nuovo silo per gli amministratori di foresta, contenente amministratori di azienda, schema e dominio. È quindi possibile configurare il silo con un criterio di autenticazione, in modo che la l'autenticazione basata su password e smart card da sistemi diversi dai controller di dominio e dalle console degli amministratori di dominio abbia esito negativo.

Per informazioni sulla configurazione di silo di criteri di autenticazione e di criteri di autenticazione, vedere [Come configurare gli account protetti](how-to-configure-protected-accounts.md).

### <a name="about-authentication-policy-silos"></a>Informazioni sui silo di criteri di autenticazione
Un silo di criteri di autenticazione controlla gli account che possono essere limitati dal silo e definisce i criteri di autenticazione da applicare ai membri. È possibile creare il silo in base ai requisiti specifici dell'organizzazione. I silo sono oggetti di Active Directory per utenti, computer e servizi, come definito dallo schema nella tabella seguente.

**Schema di Active Directory per silo di criteri di autenticazione**

|Nome visualizzato|Descrizione|
|--------|--------|
|Authentication Policy Silo|Un'istanza di questa classe definisce i criteri di autenticazione e i comportamenti correlati per gli utenti, i computer e i servizi assegnati.|
|Authentication Policy Silos|Un contenitore di questa classe può includere oggetti di silo di criteri di autenticazione.|
|Authentication Policy Silo Enforced|Specifica se il silo di criteri di autenticazione è applicato.<br /><br />Se non è applicato, per impostazione predefinita il criterio si trova in modalità di controllo. Sono generati eventi che indicano operazioni potenzialmente riuscite e non riuscite, ma al sistema non è applicata alcuna protezione.|
|Assigned Authentication Policy Silo Backlink|Questo attributo è il back link per msDS-AssignedAuthNPolicySilo.|
|Authentication Policy Silo Members|Specifica quali entità sono assegnate ad AuthNPolicySilo.|
|Authentication Policy Silo Members Backlink|Questo attributo è il back link per msDS-AuthNPolicySiloMembers.|

I silo di criteri di autenticazione possono essere configurati usando la console di amministrazione di Active Directory o Windows PowerShell. Per altre informazioni, vedere [Come configurare gli account protetti](how-to-configure-protected-accounts.md).

### <a name="about-authentication-policies"></a>Informazioni sui criteri di autenticazione
Un criterio di autenticazione definisce le proprietà relative alla durata del ticket TGT (Ticket-Granting Ticket) del protocollo Kerberos e le condizioni di controllo di accesso di autenticazione per un tipo di account. Il criterio si basa su e controlla il contenitore di Active Directory Domain Services, noto come silo di criteri di autenticazione.

I criteri di autenticazione controllano gli elementi seguenti:

-   La durata TGT per l'account, che è impostato su non rinnovabile.

-   I criteri che gli account di dispositivo devono soddisfare per effettuare l'accesso con una password o un certificato.

-   I criteri che utenti e dispositivi devono soddisfare per l'autenticazione a servizi in esecuzione come parte dell'account.

Il tipo di account Active Directory determina il ruolo del chiamante come uno dei seguenti:

-   **Utente**

    Gli utenti devono essere sempre membri del gruppo di sicurezza Utenti protetti, che rifiuta per impostazione predefinita i tentativi di autenticazione tramite NTLM.

    I criteri possono essere configurati per impostare la durata TGT di un account utente su un valore più breve o limitare i dispositivi a cui può accedere un account utente. Possono configurare espressioni avanzate nei criteri di autenticazione per controllare i criteri che gli utenti e i dispositivi devono soddisfare per l'autenticazione al servizio.

    Per altre informazioni, vedere [Gruppo di sicurezza Utenti protetti](protected-users-security-group.md).

-   **Service**

    Sono usati account del servizio gestiti autonomi, account del servizio gestiti del gruppo oppure un oggetto account personalizzato derivato da questi due tipi di account del servizio. Impostare criteri accesso al dispositivo condizioni di controllo, vengono utilizzate per limitare le credenziali dell'account del servizio gestito a specifici dispositivi con un'identità di Active Directory. I servizi non devono essere mai membri del gruppo di sicurezza Utenti protetti, poiché tutte le autenticazioni in ingresso avranno esito negativo.

-   **Computer**

    Viene usato l'oggetto account del computer o l'oggetto account personalizzato derivato dall'oggetto account del computer. Mediante i criteri è possibile impostare le condizioni di controllo di accesso necessarie per permettere l'autenticazione all'account in base alle proprietà di utente e dispositivo. I computer non devono essere mai membri del gruppo di sicurezza Utenti protetti, poiché tutte le autenticazioni in ingresso avranno esito negativo. Per impostazione predefinita, i tentativi di usare l'autenticazione NTLM sono respinti. È consigliabile non configurare alcuna durata TGT per account del computer.

> [!NOTE]
> È possibile impostare un criterio di autenticazione in un insieme di account senza associare i criteri a un silo di criteri di autenticazione. Questa strategia può essere usata quando si deve proteggere un singolo account.

**Schema di Active Directory per i criteri di autenticazione**

I criteri per gli oggetti di Active Directory per utenti, computer e servizi, sono definiti dallo schema nella tabella seguente.

|Type|Nome visualizzato|Descrizione|
|----|--------|--------|
|Condizione|Authentication Policy|Un'istanza di questa classe definisce i comportamenti dei criteri di autenticazione per le entità di sicurezza assegnate.|
|Condizione|Authentication Policies|Un contenitore di questa classe può includere oggetti di criteri di autenticazione.|
|Condizione|Authentication Policy Enforced|Specifica se i criteri di autenticazione sono applicati.<br /><br />Se non sono applicati, per impostazione predefinita il criterio sarà in modalità di controllo e saranno generati eventi che indicano operazioni potenzialmente riuscite e non riuscite, ma al sistema non sarà applicata alcuna protezione.|
|Condizione|Assigned Authentication Policy Backlink|Questo attributo è il back link per msDS-AssignedAuthNPolicy.|
|Condizione|Assigned Authentication Policy|Specifica quale AuthNPolicy deve essere applicato a questa entità di sicurezza.|
|Utente|User Authentication Policy|Specifica quale AuthNPolicy deve essere applicato a utenti a cui è assegnato questo oggetto silo.|
|Utente|User Authentication Policy Backlink|Questo attributo è il back link per msDS-UserAuthNPolicy.|
|Utente|ms-DS-User-Allowed-To-Authenticate-To|Questo attributo è usato per determinare l'insieme di entità di sicurezza a cui è permesso effettuare l'autenticazione a un servizio in esecuzione con l'account utente.|
|Utente|ms-DS-User-Allowed-To-Authenticate-From|Questo attributo permette di determinare l'insieme di dispositivi a cui un account utente è autorizzato ad accedere.|
|Utente|User TGT Lifetime|Specifica la durata massima di un ticket TGT Kerberos emesso a un utente (espressa in secondi). I TGT risultanti non sono rinnovabili.|
|Computer|Computer Authentication Policy|Specifica quale AuthNPolicy deve essere applicato a computer a cui è assegnato questo oggetto silo.|
|Computer|Computer Authentication Policy Backlink|Questo attributo è il back link per msDS-ComputerAuthNPolicy.|
|Computer|ms-DS-Computer-Allowed-To-Authenticate-To|Questo attributo è usato per determinare l'insieme di entità di sicurezza a cui è permesso effettuare l'autenticazione a un servizio in esecuzione con l'account computer.|
|Computer|Computer TGT Lifetime|Specifica la durata massima di un ticket TGT Kerberos emesso a un computer (espressa in secondi). La modifica di questa impostazione non è consigliata.|
|Servizio|Service Authentication Policy|Specifica quale AuthNPolicy deve essere applicato a servizi a cui è assegnato questo oggetto silo.|
|Servizio|Service Authentication Policy Backlink|Questo attributo è il back link per msDS-ServiceAuthNPolicy.|
|Servizio|ms-DS-Service-Allowed-To-Authenticate-To|Questo attributo è usato per determinare l'insieme di entità di sicurezza a cui è permesso effettuare l'autenticazione a un servizio in esecuzione con l'account del servizio.|
|Servizio|ms-DS-Service-Allowed-To-Authenticate-From|Questo attributo permette di determinare l'insieme di dispositivi a cui un account del servizio è autorizzato ad accedere.|
|Servizio|Service TGT Lifetime|Specifica la durata massima di un ticket TGT Kerberos emesso a un servizio (espressa in secondi).|

I silo di criteri di autenticazione possono essere configurati per ogni silo usando la console di amministrazione di Active Directory o Windows PowerShell. Per altre informazioni, vedere [Come configurare gli account protetti](how-to-configure-protected-accounts.md).

## <a name="how-it-works"></a>Come funziona
In questa sezione è illustrato il funzionamento dei silo di criteri di autenticazione e dei criteri di autenticazione insieme al gruppo di sicurezza Utenti protetti e all'implementazione del protocollo Kerberos in Windows.

-   [Come il protocollo Kerberos viene usato con i criteri e i silo di autenticazione](#BKMK_HowKerbUsed)

-   [Limitazione come un utente funziona l'accesso](#BKMK_HowRestrictingSignOn)

-   [Funzionamento della limitazione dell'emissione di ticket di servizio](#BKMK_HowRestrictingServiceTicket)

**account protetti**

Il gruppo di sicurezza utenti protetti attiva una protezione non configurabile sui dispositivi e computer host che eseguono Windows 8.1 e Windows Server 2012 R2 e nei controller di dominio in domini con controller di dominio primario che esegue Windows Server 2012 R2. A seconda del livello di funzionalità del dominio dell'account, i membri del gruppo di sicurezza Utenti protetti dispongono di un'ulteriore protezione grazie alle modifiche del comportamento nei metodi di autenticazione supportati in Windows.

-   Il membro del gruppo di sicurezza Utenti protetti non può autenticarsi con NTLM, l'autenticazione del digest o la delega predefinita delle credenziali CredSSP. In un dispositivo che esegue Windows 8.1 che usa uno dei seguenti Security Support Provider (SSP), l'autenticazione a un dominio avrà esito negativo quando l'account è un membro del gruppo di sicurezza utenti protetti.

-   Il protocollo Kerberos non usa i tipi di crittografia più vulnerabili DES o RC4 nel processo di preautenticazione. Ciò significa che il dominio deve essere configurato per supportare almeno il tipo di crittografia AES.

-   L'account dell'utente non è possibile usare la delega Kerberos vincolata o non vincolata delega. Ciò significa che le connessioni precedenti ad altri sistemi possono non riuscire se l'utente è membro del gruppo di sicurezza Utenti protetti.

-   L'impostazione della durata predefinita TGT (Ticket Granting Tickets), pari a quattro ore, può essere configurata con Criteri di autenticazione e silo, accessibili dal Centro di amministrazione di Active Directory. Ciò significa che, dopo che sono trascorse le quattro ore predefinite, l'utente deve ripetere l'autenticazione.

Per altre informazioni su questo gruppo di sicurezza, vedere [Come funziona il gruppo Utenti protetti](protected-users-security-group.md#BKMK_HowItWorks).

**I silo e criteri di autenticazione**

I silo di criteri di autenticazione e i criteri di autenticazione si avvalgono dell'infrastruttura di autenticazione Windows esistente. L'uso del protocollo NTLM è rifiutato ed è usato il protocollo Kerberos con tipi di crittografia più recenti. I criteri di autenticazione integrano il gruppo di sicurezza Utenti protetti, offrendo un modo per applicare restrizioni configurabili agli account, oltre a offrire restrizioni per gli account per servizi e computer. I criteri di autenticazione sono applicati durante lo scambio del servizio di autenticazione del protocollo Kerberos o del ticket TGS (Ticket-Granting Service). Per altre informazioni sul modo in cui Windows usa il protocollo Kerberos e sulle modifiche apportate per supportare i silo di criteri di autenticazione e i criteri di autenticazione, vedere:

-   [Come funziona il protocollo Kerberos versione 5](https://technet.microsoft.com/library/cc772815(v=ws.10).aspx)

-   [Modifiche all'autenticazione Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) (Windows Server 2008 R2 e Windows 7)

### <a name="BKMK_HowKerbUsed"></a>Come il protocollo Kerberos viene usato con i criteri e silo di criteri di autenticazione
Quando un account di dominio è collegato a un silo di criteri di autenticazione e l'utente effettua l'accesso, Gestione account di protezione aggiunge il tipo di attestazione del silo di criteri di autenticazione che include il silo come valore. L'attestazione relativa all'account offre l'accesso al silo di destinazione.

Quando un criterio di autenticazione è applicato e nel controller di dominio si riceve una richiesta del servizio di autenticazione per un account di dominio, il controller di dominio restituisce un TGT non rinnovabile con la durata configurata, a meno che la durata TGT del dominio non sia inferiore.

> [!NOTE]
> L'account di dominio deve avere una durata TGT configurata e deve essere collegato direttamente al criterio o collegato indirettamente tramite l'appartenenza al silo.

Quando un criterio di autenticazione è in modalità di controllo e nel controller di dominio si riceve la richiesta del servizio di autenticazione per un account di dominio, il controller di dominio verifica se l'autenticazione è consentita per il dispositivo, in modo da potere registrare un avviso in caso di errore. Un criterio di autenticazione controllato non altera il processo, quindi le richieste di autenticazione non avranno esito negativo se non soddisfano i requisiti del criterio.

> [!NOTE]
> L'account di dominio deve essere collegato direttamente al criterio o collegato indirettamente tramite l'appartenenza al silo.

Quando un criterio di autenticazione è applicato e il servizio di autenticazione è "blindato", la richiesta del servizio di autenticazione per un account di dominio viene ricevuta nel controller di dominio, che verifica se l'autenticazione è consentita per il dispositivo. In caso di esito negativo, il controller di dominio restituisce un messaggio di errore e registra un evento.

> [!NOTE]
> L'account di dominio deve essere collegato direttamente al criterio o collegato indirettamente tramite l'appartenenza al silo.

Quando un criterio di autenticazione è in modalità di controllo e viene ricevuta una richiesta di servizio di concessione ticket dal controller di dominio per un account di dominio, il controller di dominio verifica se l'autenticazione è consentita basato su ticket della richiesta certificato attributi privilegi I dati (PAC) che registra un messaggio di avviso in caso di errore. Il certificato attributi privilegi include diversi tipi di dati relativi all'autorizzazione, inclusi i gruppi a cui appartiene l'utente, i diritti di cui dispone l'utente e i criteri applicabili all'utente. Queste informazioni vengano utilizzate per generare token di accesso dell'utente. Se si tratta di un criterio di autenticazione applicato che permette l'autenticazione a un utente, dispositivo o servizio, il controller di dominio verifica se è consentita l'autenticazione basata su ticket della richiesta dei dati su certificato attributi privilegi. In caso di esito negativo, il controller di dominio restituisce un messaggio di errore e registra un evento.

> [!NOTE]
> L'account di dominio deve essere collegato direttamente oppure tramite l'appartenenza a un silo a un criterio di autenticazione controllato, che permette l'autenticazione a un utente, un dispositivo o un servizio.

È possibile usare un singolo criterio di autenticazione per tutti i membri di un silo oppure usare criteri distinti per utenti, computer e account del servizio gestiti.

I silo di criteri di autenticazione possono essere configurati per ogni silo usando la console di amministrazione di Active Directory o Windows PowerShell. Per altre informazioni, vedere [Come configurare gli account protetti](how-to-configure-protected-accounts.md).

### <a name="BKMK_HowRestrictingSignOn"></a>Limitazione come un utente funziona l'accesso
Poiché questi criteri di autenticazione sono applicati a un account, sono applicabili anche agli account usati dai servizi. Questa impostazione è utile per limitare l'uso di una password per un servizio ad alcuni host specifici. Ad esempio, è possibile configurare account del servizio gestiti per un gruppo in modo che gli host siano autorizzati a recuperare la password da Servizi di dominio Active Directory. La password può essere però usata da qualsiasi host per l'autenticazione iniziale. Tramite l'applicazione di una condizione di controllo dell'accesso, è possibile ottenere un livello aggiuntivo di protezione limitando la password solo all'insieme di host in grado di recuperare la password.

Servizi che vengono eseguiti come sistema, servizio di rete o altre identità del servizio locale si connettono ai servizi di rete, Usa account del computer dell'host. Non è possibile applicare restrizioni agli account computer. Anche se il servizio usa un account computer non relativo a un host Windows , non sarà quindi possibile applicarvi restrizioni.

Limitazione dell'accesso dell'utente a host specifici richiede il controller di dominio per convalidare l'identità dell'host. Quando si usa l'autenticazione Kerberos con la blindatura Kerberos (che fa parte del Controllo dinamico degli accessi), al Centro distribuzione chiavi viene fornito il TGT dell'host da cui l'utente esegue l'autenticazione. Il contenuto del TGT blindato è usato per completare una verifica dell'accesso, in modo da determinare se l'host è autorizzato.

Quando un utente accede a Windows o inserisce le credenziali per il dominio in una richiesta di credenziali per un'applicazione, per impostazione predefinita Windows invia una richiesta AS-REQ blindata al controller di dominio. Se l'utente invia la richiesta da un computer che non supporta la blindatura, ad esempio i computer che eseguono Windows 7 o Windows Vista, la richiesta ha esito negativo.

Il processo è descritto nell'elenco seguente:

-   Il controller di dominio in un dominio che esegue Windows Server 2012 R2 esegue una query per l'account utente e determina se viene configurato con un criterio di autenticazione che limita l'autenticazione iniziale che richiede richieste blindate.

-   La richiesta avrà esito negativo nel controller di dominio.

-   Poiché la blindatura è obbligatorio, l'utente può tentare di accedere usando un computer che eseguono Windows 8.1 o Windows 8, che è abilitato per supportare la blindatura Kerberos per ripetere il processo di accesso.

-   Windows rileva che il dominio supporta la blindatura Kerberos e invia una richiesta AS-REQ blindata per riprovare la richiesta di accesso.

-   Il controller di dominio esegue una verifica dell'accesso tramite le condizioni di controllo di accesso configurate e le informazioni sull'identità del sistema operativo client disponibili nel TGT usato per la blindatura della richiesta.

-   In caso di errore della verifica dell'accesso, il controller di dominio respinge la richiesta.

Anche se i sistemi operativi supportano la blindatura Kerberos, è possibile applicare i requisiti per il controllo di accesso, che devono essere soddisfatti prima che sia concesso l'accesso. Gli utenti accedono a Windows o immettono le credenziali del dominio in una richiesta di credenziali per un'applicazione. Per impostazione predefinita, Windows invia una richiesta AS-REQ non blindata al controller di dominio. Se l'utente invia la richiesta da un computer che supporta la blindatura, ad esempio Windows 8.1 o Windows 8, i criteri di autenticazione vengono valutati come indicato di seguito:

1.  Il controller di dominio in un dominio che esegue Windows Server 2012 R2 esegue una query per l'account utente e determina se viene configurato con un criterio di autenticazione che limita l'autenticazione iniziale che richiede richieste blindate.

2.  Il controller di dominio esegue una verifica dell'accesso tramite le condizioni di controllo di accesso configurate e le informazioni sull'identità di sistema disponibili nel TGT usato per la blindatura della richiesta. La verifica dell'accesso ha esito positivo.

    > [!NOTE]
    > Se sono configurate restrizioni legacy per il gruppo di lavoro, sarà necessario soddisfarle.

3.  Il controller di dominio risponde con una risposta blindata (AS-REP) e il processo di autenticazione continua.

### <a name="BKMK_HowRestrictingServiceTicket"></a>Funzionamento della limitazione dell'emissione di ticket di servizio
Quando non è consentito un account e un utente con TGT tenta di connettersi al servizio (ad esempio aprendo un'applicazione che richiede l'autenticazione a un servizio identificato dal nome dell'entità servizio del servizio (SPN), si verifica la sequenza seguente:

1.  In caso di tentativo di connessione a SPN1 da SPN, Windows invia una richiesta TGS-REQ al controller di dominio che richiede un ticket di servizio per SPN1.

2.  Il controller di dominio in un dominio che esegue Windows Server 2012 R2 Cerca SPN1 per trovare l'account di Active Directory Domain Services per il servizio e determina che l'account è configurato con un criterio di autenticazione che limita l'emissione di ticket di servizio.

3.  Il controller di dominio esegue una verifica dell'accesso tramite le condizioni di controllo di accesso configurate e le informazioni sull'identità dell'utente disponibili nel TGT. La verifica dell'accesso ha esito negativo.

4.  Il controller di dominio respinge la richiesta.

Quando un account è consentito perché l'account soddisfi le condizioni di controllo di accesso impostati dal criterio di autenticazione e un utente con TGT tenta di connettersi al servizio (ad esempio aprendo un'applicazione che richiede l'autenticazione a un servizio vale a dire identificato dal nome SPN del servizio), si verifica la sequenza seguente:

1.  In caso di tentativo di connessione a SPN1, Windows invia una richiesta TGS-REQ al controller di dominio che richiede un ticket di servizio per SPN1.

2.  Il controller di dominio in un dominio che esegue Windows Server 2012 R2 Cerca SPN1 per trovare l'account di Active Directory Domain Services per il servizio e determina che l'account è configurato con un criterio di autenticazione che limita l'emissione di ticket di servizio.

3.  Il controller di dominio esegue una verifica dell'accesso tramite le condizioni di controllo di accesso configurate e le informazioni sull'identità dell'utente disponibili nel TGT. La verifica dell'accesso ha esito positivo.

4.  Il controller di dominio risponde alla richiesta con una richiesta TGS (TGS-REP).

## <a name="BKMK_ErrorandEvents"></a>Errore associato e i messaggi di evento informativo
Nella tabella seguente sono descritti gli eventi associati al gruppo di sicurezza Utenti protetti e i criteri di autenticazione applicati ai silo di criteri di autenticazione.

Gli eventi sono registrati nei registri relativi ad applicazioni e servizi in **Microsoft\Windows\Authentication**.

Per procedure di risoluzione dei problemi che usano questi eventi, vedere [Risolvere i problemi relativi ai criteri di autenticazione](how-to-configure-protected-accounts.md#troubleshoot-authentication-policies) e [Risolvere i problemi degli eventi relativi a Utenti protetti](how-to-configure-protected-accounts.md#troubleshoot-events-related-to-protected-users).

|ID e registro eventi|Descrizione|
|----------|--------|
|101<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Un errore di accesso NTLM si verifica perché il criterio di autenticazione è configurato.<br /><br />Un evento viene registrato nel controller di dominio per indicare che l'autenticazione NTLM non è riuscita perché sono necessarie restrizioni per il controllo di accesso e queste restrizioni non sono applicabili a NTLM.<br /><br />Mostra i nomi di account, dispositivo, criterio e silo.|
|105<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Si verifica un errore di restrizioni Kerberos perché l'autenticazione da un dispositivo specifico non è stata permessa.<br /><br />Un evento viene registrato nel controller di dominio per indicare che un TGT Kerberos è stato rifiutato perché il dispositivo non soddisfa le restrizioni applicate per il controllo di accesso.<br /><br />Mostra i nomi di account, dispositivo, criterio e silo e la durata TGT.|
|305<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: È possibile che si verifichi un errore relativo alle restrizioni Kerberos perché l'autenticazione da un dispositivo specifico non è stata permessa.<br /><br />In modalità di controllo, un evento informativo viene registrato nel controller di dominio per determinare se un TGT Kerberos sarà rifiutato poiché il dispositivo non soddisfa le restrizioni applicate per il controllo di accesso.<br /><br />Mostra i nomi di account, dispositivo, criterio e silo e la durata TGT.|
|106<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Si verifica un errore di restrizioni Kerberos perché l'utente o il dispositivo non è stato autorizzato all'autenticazione sul server.<br /><br />Un evento viene registrato nel controller di dominio per indicare che un ticket di servizio Kerberos è stato rifiutato perché l'utente, il dispositivo o entrambi non soddisfano le restrizioni applicate per il controllo di accesso.<br /><br />Mostra i nomi di dispositivo, criterio e silo.|
|306<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: È possibile che si verifichi un errore relativo alle restrizioni Kerberos perché l'utente o il dispositivo non è stato autorizzato all'autenticazione sul server.<br /><br />In modalità di controllo, un evento informativo viene registrato nel controller di dominio per indicare che un ticket di servizio Kerberos sarà rifiutato poiché l'utente, il dispositivo o entrambi non soddisfano le restrizioni applicate per il controllo di accesso.<br /><br />Mostra i nomi di dispositivo, criterio e silo.|

## <a name="see-also"></a>Vedere anche
[Come configurare gli account protetti](how-to-configure-protected-accounts.md)

[Gestione e protezione delle credenziali](credentials-protection-and-management.md)

[Gruppo di sicurezza Utenti protetti](protected-users-security-group.md)


