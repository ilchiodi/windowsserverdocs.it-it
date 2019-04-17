---
ms.assetid: 70c99703-ff0d-4278-9629-b8493b43c833
title: Come configurare gli account protetti
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4de7b1c9e3d12556f67c4515467bccd124e5e73e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="how-to-configure-protected-accounts"></a>Come configurare gli account protetti

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Tramite gli attacchi Pass-the-hash (PtH), un utente malintenzionato può autenticarsi a un server remoto o il servizio utilizzando l'hash NTLM sottostante della password di un utente (o altri derivati delle credenziali). Microsoft has previously [published guidance](https://www.microsoft.com/download/details.aspx?id=36036) to mitigate pass-the-hash attacks.  Windows Server 2012 R2 include nuove funzionalità per limitare ulteriormente tali attacchi. For more information about other security features that help protect against credential theft, see [Credentials Protection and Management](https://technet.microsoft.com/library/dn408190.aspx). In questo argomento viene illustrato come configurare le nuove funzionalità seguenti:

-   [Utenti protetti](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_AddtoProtectedUsers)

-   [Criteri di autenticazione](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_CreateAuthNPolicies)

-   [Silo di criteri di autenticazione](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_CreateAuthNPolicySilos)

Sono disponibili opzioni di prevenzione aggiuntive integrate in Windows 8.1 e Windows Server 2012 R2 per la protezione contro il furto di credenziali, che sono illustrati negli argomenti seguenti:

-   [Modalità di amministrazione limitata per Desktop remoto](http://blogs.technet.com/b/kfalde/archive/2013/08/14/restricted-admin-mode-for-rdp-in-windows-8-1-2012-r2.aspx)

-   [Protezione LSA](https://technet.microsoft.com/library/dn408187)

## <a name="BKMK_AddtoProtectedUsers"></a>Utenti protetti
Utenti protetti è un nuovo gruppo di sicurezza globale a cui è possibile aggiungere utenti nuovi o esistenti. I dispositivi Windows 8.1 e Windows Server 2012 R2 host hanno un comportamento speciale con i membri di questo gruppo per fornire una migliore protezione contro il furto di credenziali. Per un membro del gruppo, un host Windows Server 2012 R2 o un dispositivo Windows 8.1 non memorizzare nella cache le credenziali che non sono supportate per utenti protetti. I membri di questo gruppo non dispongono di alcuna protezione aggiuntiva se sono connessi a un dispositivo che esegue una versione di Windows precedenti a Windows 8.1.

Gruppo di utenti protetti che sono firmati-on per i dispositivi Windows 8.1 e Windows Server 2012 R2 host può *non è più* utilizzare:

-   Delega delle credenziali (CredSSP) - predefinita credenziali non vengono memorizzate nella cache in testo normale, anche se il **Consenti delega credenziali predefinite** criterio è abilitato

-   Digest di Windows - testo normale credenziali non sono memorizzate nella cache anche quando sono abilitate

-   NTLM - NTOWF non viene memorizzato nella cache

-   Chiavi a lungo termine Kerberos - Kerberos ticket di concessione ticket (TGT) viene acquisito all'accesso e non può essere riacquisito automaticamente

-   Accesso non in linea: il verificatore dell'accesso memorizzato nella cache non viene creato

Se il livello di funzionalità del dominio è Windows Server 2012 R2, i membri del gruppo non possono più:

-   Eseguire l'autenticazione con l'autenticazione NTLM

-   Utilizzare i pacchetti di crittografia DES Data Encryption Standard () o RC4 nella preautenticazione Kerberos

-   Essere delegati tramite delega vincolata o non

-   Rinnovare i ticket utente (TGT) oltre la durata iniziale di 4 ore

To add users to the group, you can use [UI tools](https://technet.microsoft.com/library/cc753515.aspx) such as Active Directory Administrative Center (ADAC) or Active Directory Users and Computers, or a command-line tool such as [Dsmod group](https://technet.microsoft.com/library/cc732423.aspx), or the Windows PowerShell[Add-ADGroupMember](https://technet.microsoft.com/library/ee617210.aspx) cmdlet. Gli account per servizi e computer *non dovrebbero essere* essere membri del gruppo utenti protetti. L'appartenenza per questi account non fornisce protezioni locali perché la password o il certificato è sempre disponibile nell'host.

> [!WARNING]
> Le restrizioni dell'autenticazione sono disponibili soluzioni alternative, il che significa che i membri di gruppi con privilegi elevati, ad esempio il gruppo Enterprise Admins o del gruppo Domain Admins sono soggetti alle stesse restrizioni degli altri membri del gruppo utenti protetti. Se tutti i membri di tali gruppi vengono aggiunti al gruppo utenti protetti, è possibile che questi account vengano bloccati. Non aggiungere mai tutti gli account con privilegi elevati al gruppo utenti protetti fino a quando non è stato testato accuratamente il potenziale impatto.

Membri del gruppo utenti protetti devono essere in grado di eseguire l'autenticazione tramite Kerberos con Advanced Encryption Standard (AES). Questo metodo richiede le chiavi AES per l'account in Active Directory. L'account predefinito Administrator non ha una chiave AES a meno che la password è stata modificata in un controller di dominio che esegue Windows Server 2008 o versione successiva. Inoltre, qualsiasi account la cui password è stata modificata in un controller di dominio che esegue una versione precedente di Windows Server, è bloccato. Di conseguenza, segui queste procedure consigliate:

-   Non eseguire test in domini a meno che non **tutti i controller di dominio eseguono Windows Server 2008 o versione successiva**.

-   **Modifica della password** per tutti gli account di dominio che sono stati creati *prima* il dominio è stato creato. In caso contrario, questi account non possono essere autenticati.

-   **Modifica della password** per ogni utente prima di aggiungere l'account per utenti protetti di gruppo o verificare che la password sia stata modificata di recente in un controller di dominio che esegue Windows Server 2008 o versione successiva.

### <a name="BKMK_Prereq"></a>Requisiti per l'utilizzo di account protetti
Gli account protetti sono i requisiti di distribuzione seguenti:

-   Per specificare restrizioni sul lato client per utenti protetti, gli host devono eseguire Windows 8.1 o Windows Server 2012 R2. Un utente deve solo sign-on con un account che sia membro di un gruppo utenti protetti. In this case, the Protected Users group can be created by [transferring the primary domain controller (PDC) emulator role](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx) to a domain controller that runs  Windows Server 2012 R2 . Dopo tale oggetto gruppo viene replicato in altri controller di dominio, il ruolo emulatore PDC può essere ospitato in un controller di dominio che esegue una versione precedente di Windows Server.

-   Per specificare restrizioni sul lato controller di dominio per utenti protetti, che consiste nel limitare l'utilizzo dell'autenticazione NTLM, e altre restrizioni, il livello di funzionalità del dominio deve essere Windows Server 2012 R2. Per ulteriori informazioni sui livelli di funzionalità, vedere [livelli di funzionalità di servizi di dominio Active Directory informazioni (AD DS)](../active-directory-functional-levels.md).

### <a name="BKMK_TrubleshootingEvents"></a>Risolvere i problemi relativi a eventi correlati a utenti protetti
Questa sezione vengono descritti i nuovi registri per risolvere gli eventi correlati a utenti protetti e come utenti protetti può influire sulle modifiche per problemi di uno dei due ticket di concessione ticket (TGT) delega o alla scadenza.

#### <a name="new-logs-for-protected-users"></a>Nuovi registri per utenti protetti

Due nuovi registri amministrativi sono disponibili per la risoluzione di eventi correlati a utenti protetti: utenti protetti - Client Log e Protected User Failures - Domain Controller Log. Questi nuovi registri si trovano nel Visualizzatore eventi e sono disabilitati per impostazione predefinita. Per abilitare un registro, fare clic su **registri applicazioni e servizi**, fare clic su **Microsoft**, fare clic su **Windows**, fare clic su **autenticazione**, fare clic sul nome del log e fare clic su **azione** (o fare doppio clic sul registro) e fare clic su **Attiva registro**.

For more information about events in these logs, see [Authentication Policies and Authentication Policy Silos](https://technet.microsoft.com/library/dn486813.aspx).

#### <a name="troubleshoot-tgt-expiration"></a>Risolvere i problemi relativi a scadenza TGT
In genere, il controller di dominio imposta la durata TGT e il rinnovo basato su criteri di dominio, come illustrato nella finestra dell'Editor Gestione criteri di gruppo seguente.

![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTExpiration.png)

Per **utenti protetti**, le impostazioni seguenti sono hardcoded:

-   Durata massima ticket utente: 240 minuti

-   Durata massima rinnovo ticket utente: 240 minuti

#### <a name="troubleshoot-delegation-issues"></a>Risolvere i problemi di delega
In precedenza, se una tecnologia che usa la delega Kerberos è stata esito negativo, l'account del client è stato controllato per vedere se **Account è sensibile e non può essere delegato** è stato impostato. Tuttavia, se l'account è membro di **utenti protetti**, potrebbe non avere questa impostazione configurata in Active Directory amministrativi centro. Di conseguenza, controllare l'impostazione e l'appartenenza al gruppo per la risoluzione dei problemi di delega.

![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TshootDelegation.gif)

### <a name="BKMK_AuditAuthNattempts"></a>Controllare i tentativi di autenticazione
Per controllare in modo esplicito i tentativi di autenticazione per i membri del **utenti protetti** gruppo, è possibile continuare a raccogliere eventi di controllo del Registro di protezione o raccogliere i dati nei nuovi registri amministrativi. For more information about these events, see [Authentication Policies and Authentication Policy Silos](https://technet.microsoft.com/library/dn486813.aspx)

### <a name="BKMK_ProvidePUdcProtections"></a>Fornire protezioni sul lato controller di dominio per servizi e computer
Gli account per servizi e computer non possono essere membri di **utenti protetti**. In questa sezione vengono illustrate le protezioni basate sul controller di dominio possono essere disponibili per questi account:

-   Reject NTLM authentication: Only configurable via [NTLM block policies](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

-   Rifiutare Data Encryption Standard (DES) nella preautenticazione Kerberos: il controller di dominio di Windows Server 2012 R2 non accettano la crittografia DES per gli account computer a meno che non sono configurati per DES solo perché ogni versione di Windows rilasciata con Kerberos supporta anche RC4.

-   Rifiutare la crittografia RC4 nella preautenticazione Kerberos: non configurabile.

    > [!NOTE]
    > Sebbene sia possibile [modificare la configurazione dei tipi di crittografia supportati](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx), è consigliabile non modificare le impostazioni per gli account computer senza eseguire il test nell'ambiente di destinazione.

-   Limitare i ticket utente (TGT) per una durata iniziale di 4 ore: usare criteri di autenticazione.

-   Negare la delega vincolata o non vincolata: per limitare un account, aprire Active Directory amministrativi centro e selezionare il **Account è sensibile e non può essere delegato** casella di controllo.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TshootDelegation.gif)

## <a name="BKMK_CreateAuthNPolicies"></a>Criteri di autenticazione
Criteri di autenticazione è un nuovo contenitore di dominio Active Directory che contiene gli oggetti Criteri di autenticazione. Criteri di autenticazione è possono specificare le impostazioni che consentono di ridurre l'esposizione al furto di credenziali, ad esempio la limitazione della durata TGT per gli account o l'aggiunta di altre condizioni basate su attestazioni.

In Windows Server 2012, controllo dinamico degli accessi ha introdotto una classe di oggetti di ambito della foresta di Active Directory denominata criteri di accesso centrale per fornire un modo semplice per configurare file server all'interno dell'organizzazione. In Windows Server 2012 R2, una nuova classe di oggetti denominata criteri di autenticazione (objectClass msDS-AuthNPolicies) può essere usata per applicare la configurazione di autenticazione alle classi di account in domini di Windows Server 2012 R2. Classi di account Active Directory sono:

-   Utente

-   Computer

-   Account del servizio gestito e gruppo di Account del servizio gestito (GMSA)

### <a name="quick-kerberos-refresher"></a>Aggiornamento rapido Kerberos
Il protocollo di autenticazione Kerberos è costituito da tre tipi di scambi, noti anche come protocolli secondari:

![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_KerbRefresher.gif)

-   Lo scambio di servizio di autenticazione (krb_as _ *)

-   Scambio del servizio (TGS) il Ticket di concessione (krb_tgs _ *)

-   Lo scambio Client/Server (AP) (krb_ap _ *)

Lo scambio AS è in cui il client utilizza la password dell'account o la chiave privata per creare un preautenticatore e richiedere un ticket di concessione ticket (TGT). Questo avviene all'accesso utente o la prima volta che è necessario un ticket di servizio.

Lo scambio TGS è dove TGT dell'account viene utilizzato per creare un autenticatore e richiedere un ticket di servizio. Questo accade quando è necessaria una connessione autenticata.

Lo scambio client / server avviene normalmente nei dati all'interno del protocollo dell'applicazione e non è interessato dai criteri di autenticazione.

Per ulteriori informazioni, vedere [Kerberos versione 5 autenticazione protocollo funzionamento](https://technet.microsoft.com/library/cc772815(v=WS.10).aspx).

### <a name="overview"></a>Panoramica
Criteri di autenticazione integrano utenti protetti, offrendo un modo per applicare restrizioni configurabili agli account oltre a offrire restrizioni per gli account per servizi e computer. Criteri di autenticazione sono applicati durante lo scambio AS o TGS exchange.

È possibile limitare l'autenticazione iniziale o lo scambio AS configurando:

-   Durata TGT

-   Condizioni di controllo di accesso per limitare l'utente sign-on, che devono essere soddisfatte dai dispositivi da cui proviene lo scambio AS

![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictAS.gif)

È possibile limitare le richieste di ticket di servizio tramite uno scambio di servizio (TGS) concessione ticket tramite la configurazione:

-   Condizioni di controllo di accesso che devono essere soddisfatte dal client (utente, servizio, computer) o un dispositivo da cui proviene lo scambio TGS

### <a name="BKMK_ReqForAuthnPolicies"></a>Requisiti per l'utilizzo di criteri di autenticazione

|Criteri|Requisiti|
|----------|----------------|
|Specifica durate TGT personalizzate| Domini account con livello funzionale di dominio Windows Server 2012 R2|
|Limitare l'accesso utente|-Domini account con livello funzionale di dominio Windows Server 2012 R2 con supporto per controllo dinamico degli accessi<br />-Supportano Windows 8, Windows 8.1, Windows Server 2012 o Windows Server 2012 R2 dispositivi con controllo dinamico degli accessi|
|Limita il rilascio di ticket di servizio che è in base ai gruppi di sicurezza e account utente| Domini di risorse a livello funzionale di dominio Windows Server 2012 R2|
|Limita il rilascio di ticket di servizio in base alle attestazioni utente o account del dispositivo, gruppi di sicurezza o attestazioni| Domini di risorse a livello funzionale di dominio di Windows Server 2012 R2 con supporto per controllo dinamico degli accessi|

### <a name="restrict-a-user-account-to-specific-devices-and-hosts"></a>Limitare un account utente a dispositivi e host specifici
Un account a valore elevato con privilegi amministrativi deve essere un membro del **utenti protetti** gruppo. Per impostazione predefinita, nessun account è membro del **utenti protetti** gruppo. Prima di aggiungere account al gruppo, configurare il supporto di controller di dominio e creare un criterio di controllo per assicurarsi che non siano presenti problemi di blocchi.

#### <a name="configure-domain-controller-support"></a>Configurare il supporto di controller di dominio

Il dominio dell'account utente deve essere a livello funzionale di dominio Windows Server 2012 R2 (livello). Ensure all the domain controllers are  Windows Server 2012 R2 , and then use Active Directory Domains and Trusts to [raise the DFL](https://technet.microsoft.com/library/cc753104.aspx) to  Windows Server 2012 R2 .

**Per configurare il supporto per controllo dinamico degli accessi**

1.  Nel criterio controller di dominio predefinito, fare clic su **abilitato** per abilitare **supporto client Centro distribuzione chiavi (KDC) di attestazioni, autenticazione composta e blindatura Kerberos** in configurazione Computer | Modelli amministrativi | Sistema | KDC.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_EnableKDCClaims.gif)

2.  In **opzioni**, nella casella di riepilogo a discesa, selezionare **Fornisci sempre attestazioni**.

    > [!NOTE]
    > **Supportato** può anche essere configurato, ma poiché il dominio è Windows Server 2012 R2 DFL, con i controller di dominio sempre fornire attestazioni consentirà l'accesso basato sulle attestazioni utente si verificano quando si utilizzano dispositivi grado di riconoscere attestazioni non controlla e dagli host per connettersi ai servizi in grado di riconoscere attestazioni.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AlwaysProvideClaims.png)

    > [!WARNING]
    > Configurazione **Rifiuta richieste di autenticazione non blindate** genererà errori di autenticazione da qualsiasi sistema operativo che non supporta la blindatura Kerberos, ad esempio Windows 7 e sistemi operativi precedenti, o sistemi operativi a partire da Windows 8, che non sono stati configurati in modo esplicito per supportarla.

#### <a name="create-a-user-account-audit-for-authentication-policy-with-adac"></a>Creare un controllo dell'account utente per criteri di autenticazione con centro

1.  Aprire Centro di amministrazione di Active Directory (centro).

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_OpenADAC.gif)

    > [!NOTE]
    > Selezionato **autenticazione** nodo è visibile per i domini che sono in Windows Server 2012 R2 DFL. Se non viene visualizzato il nodo, quindi riprovare utilizzando un account amministratore di dominio da un dominio in Windows Server 2012 R2 DFL.

2.  Fare clic su **criteri di autenticazione**, quindi fare clic su **New** per creare un nuovo criterio.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicy.gif)

    I criteri di autenticazione devono avere un nome visualizzato e vengono applicati per impostazione predefinita.

3.  Per creare un criterio di solo controllo, fare clic su **solo restrizioni dei criteri di controllo**.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicyAuditOnly.gif)

    Criteri di autenticazione sono applicati in base al tipo di account di Active Directory. Un singolo criterio è possibile applicare a tutti e tre i tipi di account, configurare le impostazioni per ogni tipo. Tipi di account sono:

    -   Utente

    -   Computer

    -   Account del servizio gestito di Account del servizio gestito e gruppo

    Se è stato esteso lo schema con nuove entità che può essere utilizzata per il centro distribuzione chiavi (KDC), il nuovo tipo di account viene classificato dal tipo di account derivato più vicino.

4.  Per configurare una durata TGT per gli account utente, selezionare il **specificare una durata Ticket di concessione Ticket per gli account utente** casella di controllo e specificare il tempo in minuti.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTLifetime.gif)

    Ad esempio, se si desidera una durata TGT massima di 10 ore, immettere **600** come illustrato. Se non è configurata alcuna durata TGT, quindi se l'account è membro del **utenti protetti** di gruppo, la durata TGT e rinnovo è di 4 ore. In caso contrario, rinnovo e durata TGT sono basate su criteri di dominio come illustrato nella finestra dell'Editor Gestione criteri di gruppo seguente per un dominio con le impostazioni predefinite.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTExpiration.png)

5.  Per limitare l'account utente per selezionare i dispositivi, fare clic su **modifica** per definire le condizioni necessarie per il dispositivo.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_EditAuthNPolicy.gif)

6.  Nel **Modifica condizioni di controllo di accesso** finestra, fare clic su **aggiungere una condizione**.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCondition.png)

##### <a name="add-computer-account-or-group-conditions"></a>Aggiungere condizioni account o gruppo di computer

1.  Per configurare l'account computer o gruppi, nell'elenco a discesa, selezionare la casella di riepilogo a discesa **membro di ogni** e passare alla **membro di qualsiasi**.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompMember.png)

    > [!NOTE]
    > Questo controllo di accesso definisce le condizioni del dispositivo o dell'host da cui l'utente accede. Nella terminologia di controllo di accesso, l'account computer per il dispositivo o l'host è l'utente, motivo per il quale **utente** è l'unica opzione.

2.  Fare clic su **aggiungere elementi**.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompAddItems.png)

3.  Per modificare i tipi di oggetto, fare clic su **tipi di oggetto**.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjects.gif)

4.  Per selezionare gli oggetti computer in Active Directory, fare clic su **computer**, quindi fare clic su **OK**.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjectsComputers.gif)

5.  Digitare il nome del computer per limitare l'utente e quindi fare clic su **Controlla nomi**.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjectsCompName.gif)

6.  Fare clic su OK ed eventualmente creare altre condizioni per l'account computer.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompAddConditions.png)

7.  Al termine, fare clic su **OK** e verranno visualizzate le condizioni definite per l'account computer.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompDone.png)

##### <a name="add-computer-claim-conditions"></a>Aggiungere condizioni per le attestazioni computer

1.  Per configurare le attestazioni computer, elenco a discesa gruppo per selezionare l'attestazione.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaim.gif)

    Le attestazioni sono disponibili solo se sono già viene effettuato il provisioning nella foresta.

2.  Digitare il nome dell'unità Organizzativa, l'account utente deve essere accesso limitato.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaimOUName.gif)

3.  Al termine, fare clic su OK e la casella di visualizzare le condizioni definite.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaimComplete.gif)

##### <a name="troubleshoot-missing-computer-claims"></a>Risolvere i problemi di attestazioni computer mancanti
Se l'attestazione è stato eseguito il provisioning, ma non è disponibile, è possibile che sia configurato solo per **Computer** classi.

Si supponga che si desidera limitare l'autenticazione basata sull'unità organizzativa (OU) del computer, che era già configurato, ma solo per **Computer** classi.

![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictComputers.gif)

Per l'attestazione sia disponibile per limitare l'accesso utente al dispositivo, selezionare il **utente** casella di controllo.

![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictUsersComputers.gif)

#### <a name="provision-a-user-account-with-an-authentication-policy-with-adac"></a>Eseguire il provisioning di un account utente con un criterio di autenticazione con centro

1.  Dal **utente** account, fare clic su **criteri**.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicy.gif)

2.  Selezionare il **assegnare un criterio di autenticazione per questo account** casella di controllo.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicyAssign.gif)

3.  Selezionare quindi il criterio di autenticazione da applicare all'utente.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicySelect.png)

#### <a name="configure-dynamic-access-control-support-on-devices-and-hosts"></a>Configurare il supporto di controllo dinamico degli accessi su dispositivi e host
È possibile configurare durate TGT senza configurare il controllo di accesso dinamico (DAC). Controllo dinamico degli accessi è necessaria solo per il controllo di Llowedtoauthenticatefrom e AllowedToAuthenticateTo.

Tramite criteri di gruppo o Editor criteri di gruppo locali, abilitare **supporto client Kerberos per attestazioni, autenticazione composta e blindatura Kerberos** in configurazione Computer | Modelli amministrativi | Sistema | Kerberos:

![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_KerbClientDACSupport.gif)

### <a name="BKMK_TroubleshootAuthnPolicies"></a>Risolvere i problemi relativi a criteri di autenticazione

#### <a name="determine-the-accounts-that-are-directly-assigned-an-authentication-policy"></a>Individuare gli account a cui sono assegnati direttamente un criterio di autenticazione
Nella sezione account in criterio di autenticazione sono visualizzati gli account che è sono direttamente assegnato il criterio.

![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AccountsAssigned.gif)

#### <a name="use-the-authentication-policy-failures---domain-controller-administrative-log"></a>Utilizzare Authentication Policy Failures - registro amministrativo di Controller di dominio
Un nuovo **Authentication Policy Failures - Controller di dominio** registro amministrativo in **registri applicazioni e servizi** > **Microsoft** > **Windows** > **autenticazione** è stata creata per semplificare l'individuazione degli errori dovuti ai criteri di autenticazione. Questo registro è disabilitato per impostazione predefinita. Per abilitarlo, fare doppio clic il nome del log e fare clic su **Attiva registro**. I nuovi eventi sono molto simili nel contenuto per il TGT Kerberos esistenti e ticket di servizio degli eventi di controllo. For more information about these events, see [Authentication Policies and Authentication Policy Silos](https://technet.microsoft.com/library/dn486813.aspx).

### <a name="BKMK_ManageAuthnPoliciesUsingPSH"></a>Gestire i criteri di autenticazione tramite Windows PowerShell
Questo comando crea un criterio di autenticazione denominato **TestAuthenticationPolicy**. Il **UserAllowedToAuthenticateFrom** parametro specifica i dispositivi da cui gli utenti possono eseguire l'autenticazione tramite una stringa SDDL nel file denominato someFile.txt.

```
PS C:\> New-ADAuthenticationPolicy testAuthenticationPolicy -UserAllowedToAuthenticateFrom (Get-Acl .\someFile.txt).sddl
```

Questo comando Ottiene tutti i criteri di autenticazione corrispondenti al filtro che la **filtro** parametro specifica.

```
PS C:\> Get-ADAuthenticationPolicy -Filter "Name -like 'testADAuthenticationPolicy*'" -Server Server02.Contoso.com

```

Questo comando modifica la descrizione e **UserTGTLifetimeMins** proprietà dei criteri di autenticazione specificato.

```
PS C:\> Set-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1 -Description "Description" -UserTGTLifetimeMins 45
```

Questo comando rimuove il criterio di autenticazione che il **identità** parametro specifica.

```
PS C:\> Remove-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1
```

Questo comando Usa il **Get-ADAuthenticationPolicy** cmdlet con il **filtro** parametro per ottenere tutti i criteri di autenticazione che non vengono imposte. Il set di risultati viene reindirizzato al **Remove-ADAuthenticationPolicy** cmdlet.

```
PS C:\> Get-ADAuthenticationPolicy -Filter 'Enforce -eq $false' | Remove-ADAuthenticationPolicy
```

## <a name="BKMK_CreateAuthNPolicySilos"></a>Silo di criteri di autenticazione
Silo di criteri di autenticazione è un nuovo contenitore (objectClass msDS-AuthNPolicySilos) in Active Directory per utenti, computer e gli account del servizio. Consentono di proteggere gli account di valore elevato. Mentre tutte le organizzazioni è necessario proteggere i membri dei gruppi Enterprise Admins, Domain Admins e Schema Admins, perché questi account potrebbero essere utilizzati da un utente malintenzionato per accedere a tutti gli elementi nella foresta, altri account potrebbe essere necessario anche protezione.

Alcune organizzazioni isolano i carichi di lavoro creando account univoci dedicati e applicando le impostazioni di criteri di gruppo per limitare l'accesso interattivo locale e remoto e i privilegi amministrativi. Silo di criteri di autenticazione integrano il lavoro creando un modo per definire una relazione tra l'utente, Computer e gli account del servizio gestiti. Account possono appartenere solo a un silo. È possibile configurare criteri di autenticazione per ogni tipo di account per controllare:

1.  Durata TGT non rinnovabile

2.  Accedere a condizioni di controllo per la restituzione di TGT (Nota: non si applica ai sistemi, poiché la blindatura Kerberos è obbligatorio)

3.  Condizioni di controllo di accesso per la restituzione di ticket di servizio

Inoltre, gli account in un silo di criteri di autenticazione hanno un'attestazione silo che può essere usata da risorse in grado di riconoscere attestazioni, ad esempio i file server per controllare l'accesso.

È possibile configurare un nuovo descrittore di sicurezza per controllare l'emissione di ticket di servizio in base a:

-   Utente, gruppi di sicurezza dell'utente e/o attestazioni dell'utente

-   Dispositivo, il gruppo di sicurezza del dispositivo e/o attestazioni del dispositivo

Queste informazioni ai controller di dominio della risorsa richiede controllo dinamico degli accessi:

-   Attestazioni utente:

    -   Windows 8 e versioni successive client che supporta controllo dinamico degli accessi

    -   Dominio dell'account supporta controllo dinamico degli accessi e attestazioni

-   Dispositivo e/o gruppo di sicurezza dispositivo:

    -   Windows 8 e versioni successive client che supporta controllo dinamico degli accessi

    -   Risorsa configurata per l'autenticazione composta

-   Attestazioni dispositivo:

    -   Windows 8 e versioni successive client che supporta controllo dinamico degli accessi

    -   Dominio dispositivo supporta controllo dinamico degli accessi e attestazioni

    -   Risorsa configurata per l'autenticazione composta

Criteri di autenticazione possono essere applicati a tutti i membri di un silo di criteri di autenticazione anziché a singoli account o criteri di autenticazione distinto possono essere applicati a diversi tipi di account all'interno di un silo. Ad esempio, un criterio di autenticazione può essere applicato ad account utente con privilegi elevati e criteri diversi possono essere applicati agli account del servizio. Prima di poter creare un silo di criteri di autenticazione, è necessario creare almeno un criterio di autenticazione.

> [!NOTE]
> Un criterio di autenticazione può essere applicato ai membri di un silo di criteri di autenticazione oppure applicarlo indipendentemente dai silo per limitare l'ambito dell'account specifico. Ad esempio, per proteggere un singolo account o un set limitato di account, un criterio di impostare gli account senza aggiungere gli account a un silo.

È possibile creare un silo di criteri di autenticazione utilizzando il centro di amministrazione di Active Directory o Windows PowerShell. Per impostazione predefinita, un silo di criteri di autenticazione solo i criteri del sito, che equivale a specificare il **WhatIf** parametro nel cmdlet di Windows PowerShell. In questo caso, non si applicano le restrizioni al silo criteri, ma vengono generati i controlli per indicare se si verificano errori se vengono applicate le restrizioni.

#### <a name="to-create-an-authentication-policy-silo-by-using-active-directory-administrative-center"></a>Per creare un silo di criteri di autenticazione utilizzando il centro di amministrazione di Active Directory

1.  Apri **centro di amministrazione di Active Directory**, fare clic su **autenticazione**, fare doppio clic su **silo di criteri di autenticazione**, fare clic su **New**, quindi fare clic su **Silo di criteri di autenticazione**.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CreateNewAuthNPolicySilo.gif)

2.  In **nome visualizzato**, digitare un nome per il silo. In **account consentiti**, fare clic su **Aggiungi**, digitare i nomi degli account e quindi fare clic su **OK**. È possibile specificare gli utenti, computer o gli account del servizio. Specificare quindi se usare un singolo criterio per tutte le entità o un criterio separato per ogni tipo di entità e il nome del criterio o dei criteri.

    ![account protetti](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicySiloDisplayName.gif)

### <a name="BKMK_ManageAuthnSilosUsingPSH"></a>Gestire i silo di criteri di autenticazione tramite Windows PowerShell
Questo comando crea un oggetto silo di criteri di autenticazione e lo impone.

```
PS C:\>New-ADAuthenticationPolicySilo -Name newSilo -Enforce
```

Questo comando Ottiene tutte le autenticazioni di silo di criteri che corrispondono al filtro specificato dal **filtro** parametro. L'output viene quindi passato al **Format-Table** cmdlet per visualizzare il nome del criterio e il valore per **Imponi** in ogni criterio.

```
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Name -like "*silo*"' | Format-Table Name, Enforce -AutoSize

Name  Enforce
----  -------
silo     True
silos   False

```

Questo comando Usa il **Get-ADAuthenticationPolicySilo** cmdlet con il **filtro** parametro per ottenere tutti i silo di criteri di autenticazione non imposti e pipe il risultato del filtro per il **Remove-ADAuthenticationPolicySilo** cmdlet.

```
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Enforce -eq $False' | Remove-ADAuthenticationPolicySilo
```

Questo comando concede l'accesso al silo di criteri di autenticazione denominato *Silo* per l'account utente denominato *User01*.

```
PS C:\>Grant-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01
```

Questo comando revoca l'accesso al silo di criteri di autenticazione denominato *Silo* per l'account utente denominato *User01*. Poiché il **conferma** parametro è impostato su **$False**, viene visualizzato alcun messaggio di conferma.

```
PS C:\>Revoke-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01 -Confirm:$False
```

Questo esempio Usa prima di tutto il **Get-ADComputer** cmdlet per ottenere tutti gli account computer che corrispondono al filtro che la **filtro** parametro specifica. L'output di questo comando viene passato a **Set-ADAccountAuthenticatinPolicySilo** per assegnare i silo di criteri di autenticazione denominato *Silo* e i criteri di autenticazione denominato *AuthenticationPolicy02* ad essi.

```
PS C:\>Get-ADComputer -Filter 'Name -like "newComputer*"' | Set-ADAccountAuthenticationPolicySilo -AuthenticationPolicySilo Silo -AuthenticationPolicy AuthenticationPolicy02
```



