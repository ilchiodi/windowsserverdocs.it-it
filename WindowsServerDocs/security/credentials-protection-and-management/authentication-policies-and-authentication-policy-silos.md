---
title: Criteri di autenticazione e silo di criteri di autenticazione
description: Protezione di Windows Server
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
ms.openlocfilehash: 460837c79c0e0d2c48331ddaaffcd118fd16ebc1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="authentication-policies-and-authentication-policy-silos"></a>Criteri di autenticazione e silo di criteri di autenticazione

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento destinato ai professionisti IT descrive silo di criteri di autenticazione e i criteri che possono limitare gli account in tali silo. Illustra anche come criteri di autenticazione consente di limitare l'ambito degli account.

Silo di criteri di autenticazione e i criteri corrispondenti offrono un modo per includere credenziali con privilegi elevati per sistemi che riguardano solo determinati utenti, computer o servizi. Silo possono essere definiti e gestiti in servizi di dominio Active Directory (AD DS) tramite il centro di amministrazione di Active Directory e i cmdlet di Windows PowerShell per Active Directory.

Silo di criteri di autenticazione sono contenitori a cui gli amministratori possono assegnare account utente, account computer e gli account del servizio. Gli insiemi di account possono essere quindi gestiti tramite i criteri di autenticazione che sono stati applicati a tale contenitore. Questo riduce la necessità di all'amministratore di tenere traccia dell'accesso alle risorse per gli account individuali e consente di impedire a utenti malintenzionati di accedere ad altre risorse tramite il furto di credenziali.

Le funzionalità introdotte in Windows Server 2012 R2, consentono di creare silo di criteri, che ospitano un insieme di utenti con privilegi elevati di autenticazione. È quindi possibile assegnare criteri di autenticazione per questo contenitore al limite in account con privilegi possono essere utilizzati nel dominio. Quando gli account sono nel gruppo di sicurezza utenti protetti, sono applicati controlli aggiuntivi, ad esempio l'utilizzo esclusivo del protocollo Kerberos.

Con queste funzionalità, è possibile limitare l'utilizzo di account a valore elevato di host di valore elevato. Ad esempio, è possibile creare un silo per gli amministratori nuovo insieme di strutture che contiene l'azienda, schema e gli amministratori di dominio. È quindi possibile configurare il silo con un criterio di autenticazione in modo che la password e l'autenticazione basata su smart card da sistemi diversi dai controller di dominio e console di amministrazione di dominio ha esito negativo.

Per informazioni sulla configurazione dei criteri di autenticazione e silo di criteri di autenticazione, vedere [come configurare gli account protetti](how-to-configure-protected-accounts.md).

### <a name="about-authentication-policy-silos"></a>Sui silo di criteri di autenticazione
Un silo di criteri di autenticazione controlla gli account che possono essere limitati dal silo e definisce i criteri di autenticazione da applicare ai membri. È possibile creare il silo in base ai requisiti dell'organizzazione. I silo sono oggetti Active Directory per gli utenti, computer e servizi, come definito dallo schema nella tabella seguente.

**Schema di Active Directory per silo di criteri di autenticazione**

|Nome visualizzato|Descrizione|
|--------|--------|
|Silo di criteri di autenticazione|Un'istanza di questa classe definisce i criteri di autenticazione e i comportamenti correlati per gli utenti, computer e servizi.|
|Silo di criteri di autenticazione|Un contenitore di questa classe può includere oggetti di silo di criteri di autenticazione.|
|Silo di criteri di autenticazione applicato|Specifica se viene applicato il silo di criteri di autenticazione.<br /><br />Se non è stato applicato il criterio per impostazione predefinita è in modalità di controllo. Vengono generati eventi che indicano operazioni potenzialmente riuscite e gli errori, ma le protezioni non vengono applicate al sistema.|
|Occorre Silo criteri di autenticazione assegnato|Questo attributo è il back link per msDS-AssignedAuthNPolicySilo.|
|Membri Silo di criteri di autenticazione|Specifica quali entità sono assegnate ad AuthNPolicySilo.|
|Occorre membri Silo criteri di autenticazione|Questo attributo è il back link per msDS-AuthNPolicySiloMembers.|

Silo di criteri di autenticazione possono essere configurati con la Console di amministrazione di Active Directory o Windows PowerShell. Per ulteriori informazioni, vedere [come configurare gli account protetti](how-to-configure-protected-accounts.md).

### <a name="about-authentication-policies"></a>Informazioni sui criteri di autenticazione
Un criterio di autenticazione definisce le proprietà della durata di Kerberos protocollo ticket di concessione ticket (TGT) e condizioni di controllo di accesso di autenticazione per un tipo di account. Il criterio si basa su e controlla il contenitore di dominio Active Directory noto come silo di criteri di autenticazione.

Criteri di autenticazione controllano gli elementi seguenti:

-   La durata TGT per l'account, che è impostato su non rinnovabile.

-   I criteri che gli account del dispositivo devono soddisfare per l'accesso con una password o un certificato.

-   I criteri che utenti e dispositivi devono soddisfare per l'autenticazione ai servizi in esecuzione come parte dell'account.

Il tipo di account Active Directory determina il ruolo del chiamante come uno dei seguenti:

-   **Utente**

    Gli utenti devono sempre essere membri del gruppo di sicurezza utenti protetti, che per impostazione predefinita rifiuta i tentativi di autenticazione tramite NTLM.

    Criteri possono essere configurati per impostare la durata TGT di un account utente su un valore più breve o limitare i dispositivi a cui può accedere un account utente. Le espressioni avanzata possono essere configurate in Criteri di autenticazione per controllare i criteri che gli utenti e i dispositivi devono soddisfare per l'autenticazione nel servizio.

    Per ulteriori informazioni vedere [gruppo di sicurezza utenti protetti](protected-users-security-group.md).

-   **Servizio**

    Gli account del servizio, account del servizio gestito di gruppo o un oggetto account personalizzato derivato da questi due tipi di vengono utilizzati gli account di servizio gestiti autonomi. I criteri possono impostare accesso al dispositivo condizioni di controllo, che vengono usate per limitare le credenziali dell'account del servizio gestito a specifici dispositivi con un'identità di Active Directory. Servizi non devono essere mai membri del gruppo di sicurezza utenti protetti poiché tutte le autenticazioni in ingresso avranno esito negativo.

-   **Computer**

    Viene usato l'oggetto account computer o l'oggetto account personalizzato derivato dall'oggetto account del computer. I criteri di impostare l'accesso condizioni di controllo necessarie per consentire l'autenticazione per l'account in base a utente e le proprietà di dispositivo. I computer non devono essere mai membri del gruppo di sicurezza utenti protetti poiché tutte le autenticazioni in ingresso avranno esito negativo. Per impostazione predefinita, i tentativi di usare l'autenticazione NTLM sono respinti. Durata TGT non deve essere configurata per gli account computer.

> [!NOTE]
> È possibile impostare un criterio di autenticazione in un set di account senza associare i criteri a un silo di criteri di autenticazione. Quando si dispone di un singolo account per la protezione, è possibile usare questa strategia.

**Schema di Active Directory per i criteri di autenticazione**

I criteri per gli oggetti di Active Directory per gli utenti, computer e servizi sono definiti dallo schema nella tabella seguente.

|Tipo|Nome visualizzato|Descrizione|
|----|--------|--------|
|Criteri|Criteri di autenticazione|Un'istanza di questa classe definisce i comportamenti dei criteri di autenticazione per l'entità di sicurezza assegnate.|
|Criteri|Criteri di autenticazione|Un contenitore di questa classe può includere gli oggetti Criteri di autenticazione.|
|Criteri|Criteri di autenticazione applicati|Specifica se viene applicato il criterio di autenticazione.<br /><br />Se non è stato applicato il criterio per impostazione predefinita è in modalità di controllo e vengono generati eventi che indicano operazioni potenzialmente riuscite e gli errori, ma le protezioni non vengono applicate al sistema.|
|Criteri|Occorre di criteri di autenticazione assegnato|Questo attributo è il back link per msDS-AssignedAuthNPolicy.|
|Criteri|Criteri di autenticazione assegnato|Specifica quale AuthNPolicy deve essere applicato a questa entità di sicurezza.|
|Utente|Criteri di autenticazione utente|Specifica quale AuthNPolicy deve essere applicato per gli utenti assegnati a questo oggetto silo.|
|Utente|Occorre criteri di autenticazione utente|Questo attributo è il back link per msDS-UserAuthNPolicy.|
|Utente|ms-DS-User-Allowed-To-Authenticate-To|Questo attributo è usato per determinare l'insieme di entità consentita l'autenticazione a un servizio in esecuzione con l'account utente.|
|Utente|ms-DS-User-Allowed-To-Authenticate-From|Questo attributo è usato per determinare il set di dispositivi a cui un account utente è autorizzato ad accedere.|
|Utente|Durata TGT utente|Specifica la durata massima di un TGT Kerberos emesso a un utente (espresso in secondi). I TGT risultanti non sono rinnovabili.|
|Computer|Criteri di autenticazione del computer|Specifica quale AuthNPolicy deve essere applicato a computer che vengono assegnati a questo oggetto silo.|
|Computer|Occorre criteri di autenticazione computer|Questo attributo è il back link per msDS-ComputerAuthNPolicy.|
|Computer|ms-DS-Computer-Allowed-To-Authenticate-To|Questo attributo è usato per determinare l'insieme di entità che sono autorizzati a eseguire l'autenticazione a un servizio in esecuzione con l'account computer.|
|Computer|Durata TGT di computer|Specifica la durata massima di un TGT Kerberos emesso a un computer (espresso in secondi). È consigliabile non modificare questa impostazione.|
|Servizio|Criteri di autenticazione del servizio|Specifica quale AuthNPolicy deve essere applicato ai servizi che vengono assegnati a questo oggetto silo.|
|Servizio|Occorre criteri di autenticazione del servizio|Questo attributo è il back link per msDS-ServiceAuthNPolicy.|
|Servizio|ms-DS-Service-Allowed-To-Authenticate-To|Questo attributo è usato per determinare l'insieme di entità che sono autorizzati a eseguire l'autenticazione a un servizio in esecuzione con l'account del servizio.|
|Servizio|ms-DS-Service-Allowed-To-Authenticate-From|Questo attributo è usato per determinare il set di dispositivi a cui un account del servizio è autorizzato ad accedere.|
|Servizio|Durata TGT del servizio|Specifica la durata massima di un TGT Kerberos emesso a un servizio (espresso in secondi).|

Criteri di autenticazione possono essere configurati per ogni silo usando la Console di amministrazione di Active Directory o Windows PowerShell. Per ulteriori informazioni, vedere [come configurare gli account protetti](how-to-configure-protected-accounts.md).

## <a name="how-it-works"></a>Come funziona
Questa sezione descrive il funzionano silo di criteri di autenticazione e i criteri di autenticazione in combinazione con il gruppo di sicurezza utenti protetti e l'implementazione del protocollo Kerberos in Windows.

-   [Come il protocollo Kerberos viene utilizzato con i criteri e i silo di autenticazione](#BKMK_HowKerbUsed)

-   [Limitazione come un utente funziona l'accesso](#BKMK_HowRestrictingSignOn)

-   [Funzionamento di limitare l'emissione di ticket di servizio](#BKMK_HowRestrictingServiceTicket)

**Account protetti**

Gruppo di sicurezza utenti protetti attiva una protezione non configurabile in dispositivi e computer host che eseguono Windows Server 2012 R2 e Windows 8.1 e controller di dominio in domini con un controller di dominio primario che esegue Windows Server 2012 R2. A seconda del livello funzionale di dominio dell'account, i membri del gruppo di sicurezza utenti protetti sono un'ulteriore protezione a causa delle modifiche nei metodi di autenticazione supportati in Windows.

-   Il membro del gruppo di sicurezza utenti protetti non può autenticarsi con NTLM, l'autenticazione del Digest o CredSSP la delega delle credenziali predefinite. In un dispositivo che esegue Windows 8.1 che usa uno dei seguenti Security Support Provider (SSP), l'autenticazione a un dominio avrà esito negativo se l'account è membro del gruppo di sicurezza utenti protetti.

-   Il protocollo Kerberos non usa i tipi di crittografia più vulnerabili DES o RC4 nel processo di preautenticazione. Ciò significa che il dominio deve essere configurato per supportare almeno il tipo di crittografia AES.

-   L'account dell'utente non può essere delegato con il protocollo Kerberos vincolata o non vincolata delega. Ciò significa che le connessioni precedenti ad altri sistemi possono non riuscire se l'utente è membro del gruppo di sicurezza utenti protetti.

-   L'impostazione di durata predefinita TGT Kerberos di quattro ore è configurabile tramite criteri di autenticazione e silo, accessibili tramite il centro di amministrazione di Active Directory. Ciò significa che quando è trascorso quattro ore, l'utente deve ripetere l'autenticazione.

Per ulteriori informazioni su questo gruppo di sicurezza, vedere [come gli utenti protetti gruppo funziona](protected-users-security-group.md#BKMK_HowItWorks).

**Silo e criteri di autenticazione**

Criteri di autenticazione e silo di criteri di autenticazione sfruttano l'infrastruttura di autenticazione Windows esistente. L'utilizzo del protocollo NTLM è rifiutato e viene utilizzato il protocollo Kerberos con tipi di crittografia più recenti. Criteri di autenticazione integrano il gruppo di sicurezza utenti protetti, offrendo un modo per applicare restrizioni configurabili agli account, oltre a offrire restrizioni per gli account per servizi e computer. Criteri di autenticazione sono applicati durante il servizio di autenticazione protocollo Kerberos (AS) o un ticket di concessione scambio del servizio (TGS). Per ulteriori informazioni su come Windows utilizza il protocollo Kerberos e quali modifiche sono state apportate per supportare i criteri di autenticazione e silo di criteri di autenticazione, vedere:

-   [Come funziona il protocollo Kerberos versione 5](https://technet.microsoft.com/library/cc772815(v=ws.10).aspx)

-   [Modifiche all'autenticazione Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) (Windows Server 2008 R2 e Windows 7)

### <a name="BKMK_HowKerbUsed"></a>Come il protocollo Kerberos viene utilizzato con i criteri e silo di criteri di autenticazione
Quando un account di dominio è collegato a un silo di criteri di autenticazione e l'utente effettua l'accesso, gestione account di protezione aggiunge il tipo di attestazione del Silo di criteri di autenticazione che include il silo come valore. L'attestazione relativa all'account offre l'accesso al silo di destinazione.

Quando viene applicato un criterio di autenticazione e la richiesta del servizio di autenticazione per un account di dominio viene ricevuta nel controller di dominio, il controller di dominio restituisce un TGT non rinnovabile con la durata configurata (a meno che la durata TGT di dominio più breve).

> [!NOTE]
> L'account di dominio deve avere una durata TGT configurata e deve essere direttamente collegato al criterio o collegato indirettamente tramite l'appartenenza al silo.

Quando un criterio di autenticazione è in modalità di controllo e la richiesta del servizio di autenticazione per un account di dominio viene ricevuta nel controller di dominio, il controller di dominio verifica se l'autenticazione è consentita per il dispositivo in modo da potere registrare un avviso se si verifica un errore. Un criterio di autenticazione controllato non altera il processo, in modo che le richieste di autenticazione non riuscirà se non soddisfano i requisiti dei criteri.

> [!NOTE]
> L'account di dominio deve essere collegato direttamente al criterio o collegato indirettamente tramite l'appartenenza al silo.

Quando viene applicato un criterio di autenticazione e il servizio di autenticazione è blindato, si riceve la richiesta del servizio di autenticazione per un account di dominio nel controller di dominio, il controller di dominio verifica se l'autenticazione è consentita per il dispositivo. In caso contrario, il controller di dominio restituisce un messaggio di errore e registra un evento.

> [!NOTE]
> L'account di dominio deve essere collegato direttamente al criterio o collegato indirettamente tramite l'appartenenza al silo.

Quando un criterio di autenticazione è in modalità di controllo e dal controller di dominio per un account di dominio viene ricevuta una richiesta di servizio di concessione ticket, il controller di dominio verifica se l'autenticazione è consentita in base ai dati di privilegio Attribute Certificate (PAC) del ticket della richiesta e quindi Registra un messaggio di avviso in caso di errore. Il certificato attributi privilegi include diversi tipi di dati di autorizzazione, inclusi i gruppi che l'utente è membro di, l'utente ha, i diritti e i criteri applicabili all'utente. Queste informazioni viene utilizzate per generare i token di accesso dell'utente. Se si tratta di un criterio di autenticazione applicato che permette l'autenticazione a un utente, un dispositivo o un servizio, il controller di dominio verifica se l'autenticazione è consentita in base ai dati del ticket della richiesta. In caso contrario, il controller di dominio restituisce un messaggio di errore e registra un evento.

> [!NOTE]
> L'account di dominio deve essere direttamente collegato o collegato tramite l'appartenenza al silo di criteri di autenticazione controllato, che permette l'autenticazione a un utente, un dispositivo o un servizio,

È possibile utilizzare un singolo criterio di autenticazione per tutti i membri di un silo oppure è possibile utilizzare criteri distinti per gli utenti, computer e account dei servizi gestiti.

Criteri di autenticazione possono essere configurati per ogni silo usando la Console di amministrazione di Active Directory o Windows PowerShell. Per ulteriori informazioni, vedere [come configurare gli account protetti](how-to-configure-protected-accounts.md).

### <a name="BKMK_HowRestrictingSignOn"></a>Limitazione come un utente funziona l'accesso
Poiché questi criteri di autenticazione sono applicati a un account, vale anche per gli account utilizzati da servizi. Se si desidera limitare l'uso di una password per un servizio a host specifici, questa impostazione è utile. Ad esempio, gruppo gestito service account configurati in modo in cui gli host siano autorizzati a recuperare la password da servizi di dominio Active Directory. Tuttavia, la password è utilizzabile da qualsiasi host per l'autenticazione iniziale. Applicando una condizione di controllo di accesso, è possibile ottenere un ulteriore livello di protezione limitando la password solo all'insieme di host in grado di recuperare la password.

Quando i servizi eseguiti come sistema, servizio di rete o altre identità di servizio locale si connettono ai servizi di rete, usano account computer dell'host. Gli account computer non possono essere limitati. Pertanto, anche se il servizio utilizza un account computer che non è per un host Windows, non può essere limitato.

Limitazione dell'accesso dell'utente a host specifici è necessario il controller di dominio per convalidare l'identità dell'host. Quando si utilizza l'autenticazione Kerberos con la blindatura Kerberos (che fa parte del controllo dinamico degli accessi), il centro distribuzione chiavi viene fornito il TGT dell'host da cui l'utente esegue l'autenticazione. Il contenuto del TGT blindato è usato per completare un controllo di accesso per determinare se l'host è consentito.

Quando un utente accede a Windows o immette le credenziali di dominio in una richiesta di credenziali per un'applicazione, per impostazione predefinita, Windows invia una richiesta AS-req Blindata al controller di dominio. Se l'utente invia la richiesta da un computer che non supporta la blindatura, ad esempio i computer che eseguono Windows 7 o Windows Vista, la richiesta ha esito negativo.

L'elenco seguente descrive il processo:

-   Il controller di dominio in un dominio che esegue Windows Server 2012 R2 esegue una query per l'account utente e determina se è configurato con un criterio di autenticazione che limita l'autenticazione iniziale che richiede richieste blindate.

-   Il controller di dominio avrà esito negativo della richiesta.

-   Poiché la blindatura è obbligatorio, l'utente può tentare di accedere con un computer che esegue Windows 8.1 o Windows 8, che è abilitato per il supporto della blindatura Kerberos per ritentare il processo di accesso.

-   Windows rileva che il dominio supporta la blindatura Kerberos e invia un blindati AS-REQ per ritentare la richiesta di accesso.

-   Il controller di dominio esegue una verifica dell'accesso usando le condizioni di controllo di accesso configurate e informazioni di identità del sistema operativo client nel TGT usato per la blindatura della richiesta.

-   Se il controllo di accesso non riesce, il controller di dominio respinge la richiesta.

Anche quando i sistemi operativi supportano la blindatura Kerberos, i requisiti di controllo di accesso possono essere applicati e devono essere soddisfatti prima di concessa l'accesso. Gli utenti di accedere a Windows o di immettere le credenziali di dominio in una richiesta di credenziali per un'applicazione. Per impostazione predefinita, Windows invia una richiesta AS-req Blindata al controller di dominio. Se l'utente invia la richiesta da un computer che supporta la blindatura, ad esempio Windows 8.1 o Windows 8, i criteri di autenticazione saranno valutati come segue:

1.  Il controller di dominio in un dominio che esegue Windows Server 2012 R2 esegue una query per l'account utente e determina se è configurato con un criterio di autenticazione che limita l'autenticazione iniziale che richiede richieste blindate.

2.  Il controller di dominio esegue una verifica dell'accesso usando le condizioni di controllo di accesso configurate e informazioni di identità del sistema nel TGT usato per la blindatura della richiesta. Il controllo di accesso ha esito positivo.

    > [!NOTE]
    > Se sono configurate restrizioni legacy gruppo di lavoro, quelli anche devono essere soddisfatte.

3.  Il controller di dominio risponde con una risposta blindata (AS-REP) e l'autenticazione continua.

### <a name="BKMK_HowRestrictingServiceTicket"></a>Funzionamento di limitare l'emissione di ticket di servizio
Quando un account non è consentito e un utente con TGT tenta di connettersi al servizio (ad esempio aprendo un'applicazione che richiede l'autenticazione a un servizio identificato dal nome dell'entità servizio del servizio (SPN), si verifica la sequenza seguente:

1.  Nel tentativo di connessione a SPN1 da SPN, Windows invia una richiesta TGS-REQ al controller di dominio che richiede un ticket di servizio per SPN1.

2.  Il controller di dominio in un dominio che esegue Windows Server 2012 R2 Cerca spn1 per trovare l'account servizi di dominio Active Directory per il servizio e determina che l'account è configurato con un criterio di autenticazione che limita l'emissione di ticket di servizio.

3.  Il controller di dominio esegue una verifica dell'accesso usando le condizioni di controllo di accesso configurate e le informazioni sull'identità dell'utente disponibili nel TGT. Il controllo di accesso ha esito negativo.

4.  Il controller di dominio respinge la richiesta.

Quando un account è autorizzato perché soddisfa le condizioni di controllo di accesso che vengono impostate tramite i criteri di autenticazione e un utente con TGT tenta di connettersi al servizio (ad esempio aprendo un'applicazione che richiede l'autenticazione a un servizio identificato dal nome SPN del servizio), si verifica la sequenza seguente:

1.  Nel tentativo di connessione a SPN1, Windows invia una richiesta TGS-REQ al controller di dominio che richiede un ticket di servizio per SPN1.

2.  Il controller di dominio in un dominio che esegue Windows Server 2012 R2 Cerca spn1 per trovare l'account servizi di dominio Active Directory per il servizio e determina che l'account è configurato con un criterio di autenticazione che limita l'emissione di ticket di servizio.

3.  Il controller di dominio esegue una verifica dell'accesso usando le condizioni di controllo di accesso configurate e le informazioni sull'identità dell'utente disponibili nel TGT. Il controllo di accesso ha esito positivo.

4.  Il controller di dominio risponde alla richiesta con una risposta del servizio di concessione ticket (TGS-REP).

## <a name="BKMK_ErrorandEvents"></a>Errore associato e i messaggi di evento informativo
Nella tabella seguente vengono descritti gli eventi associati al gruppo di sicurezza utenti protetti e i criteri di autenticazione che vengono applicati ai silo di criteri di autenticazione.

Gli eventi vengono registrati nei registri applicazioni e servizi in **Microsoft\Windows\Authentication**.

Per la risoluzione dei problemi che utilizzano questi eventi, vedere [risoluzione dei problemi di criteri di autenticazione](how-to-configure-protected-accounts.md#BKMK_TroubleshootAuthnPolicies) e [risolvere i problemi relativi a eventi correlati a utenti protetti](how-to-configure-protected-accounts.md#BKMK_TrubleshootingEvents).

|ID e registro eventi|Descrizione|
|----------|--------|
|101<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Un errore di accesso NTLM si verifica perché il criterio di autenticazione è configurato.<br /><br />Un evento viene registrato nel controller di dominio per indicare che l'autenticazione NTLM non è riuscita perché sono necessarie restrizioni di controllo di accesso e queste restrizioni non sono applicabili a NTLM.<br /><br />Visualizza l'account, dispositivo, criterio e silo di nomi.|
|105<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Si verifica un errore di restrizioni Kerberos perché l'autenticazione da un dispositivo specifico non è stata permessa.<br /><br />Un evento viene registrato nel controller di dominio per indicare che un TGT Kerberos è stato rifiutato perché il dispositivo non soddisfa le restrizioni di controllo di accesso applicate.<br /><br />Visualizza l'account, dispositivo, criteri, i nomi di silo e durata TGT.|
|305<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Un errore di restrizioni Kerberos potenziale potrebbe verificarsi perché l'autenticazione da un dispositivo specifico non è stata permessa.<br /><br />In modalità di controllo, un evento informativo viene registrato nel controller di dominio per determinare se un TGT Kerberos sarà rifiutato poiché il dispositivo non soddisfa le restrizioni di controllo di accesso.<br /><br />Visualizza l'account, dispositivo, criteri, i nomi di silo e durata TGT.|
|106<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Si verifica un errore di restrizioni Kerberos perché l'utente o il dispositivo non è stato autorizzato all'autenticazione al server.<br /><br />Un evento viene registrato nel controller di dominio per indicare che un ticket di servizio Kerberos è stato rifiutato perché l'utente, dispositivo o entrambi non soddisfano le restrizioni di controllo di accesso applicate.<br /><br />Visualizza il dispositivo, criterio e silo di nomi.|
|306<br /><br />**AuthenticationPolicyFailures-DomainController**|Motivo: Potrebbe verificarsi un errore di restrizioni Kerberos perché l'utente o il dispositivo non è stato autorizzato all'autenticazione al server.<br /><br />In modalità di controllo, un evento informativo viene registrato nel controller di dominio per indicare che un ticket di servizio Kerberos sarà rifiutato poiché l'utente, dispositivo o entrambi non soddisfano le restrizioni di controllo di accesso.<br /><br />Visualizza il dispositivo, criterio e silo di nomi.|

## <a name="see-also"></a>Vedere anche
[Come configurare gli account protetti](how-to-configure-protected-accounts.md)

[Gestione e protezione delle credenziali](credentials-protection-and-management.md)

[Gruppo di sicurezza utenti protetti](protected-users-security-group.md)


