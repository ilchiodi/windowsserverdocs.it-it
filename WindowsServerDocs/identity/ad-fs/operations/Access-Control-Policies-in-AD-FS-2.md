---
ms.assetid: ''
title: Criteri di controllo degli accessi client in Active Directory Federation Services 2,0
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b0d6133a6fb43b8624dc1329db632fb5dd4aa070
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358449"
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>Criteri di controllo degli accessi client in AD FS 2,0
I criteri di accesso client in Active Directory Federation Services 2,0 consentono di limitare o concedere agli utenti l'accesso alle risorse.  Questo documento descrive come abilitare i criteri di accesso client in AD FS 2,0 e come configurare gli scenari più comuni.

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>Abilitazione dei criteri di accesso client in AD FS 2,0

Per abilitare i criteri di accesso client, attenersi alla procedura riportata di seguito.

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>Passaggio 1: installare l'aggiornamento cumulativo 2 per AD FS pacchetto 2,0 nei server di AD FS

Scaricare l' [aggiornamento cumulativo 2 per il pacchetto di Active Directory Federation Services (ad FS) 2,0](https://support.microsoft.com/en-us/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0) e installarlo in tutti i server federativi e i proxy server federativi.

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>Passaggio 2: aggiungere cinque regole attestazioni all'attendibilità del provider di attestazioni Active Directory

Una volta installato l'aggiornamento cumulativo 2 in tutti i server e proxy di AD FS, usare la procedura seguente per aggiungere un set di regole attestazioni che rende disponibili i nuovi tipi di attestazioni al motore dei criteri.

A tale scopo, verranno aggiunte cinque regole di trasformazione accettazione per ognuno dei nuovi tipi di attestazioni del contesto di richiesta utilizzando la procedura seguente.

Nell'Active Directory attendibilità del provider di attestazioni, creare una nuova regola di trasformazione accettazione per passare tutti i nuovi tipi di attestazioni del contesto della richiesta.

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>Per aggiungere una regola attestazioni all'Active Directory attendibilità del provider di attestazioni per ognuno dei cinque tipi di attestazione di contesto:


1. Fare clic sul pulsante Start, scegliere programmi, strumenti di amministrazione, quindi fare clic su Gestione AD FS 2,0.
2. Nell'albero della console, in AD FS 2.0 \ relazioni di trust, fare clic su attendibilità provider di attestazioni, fare clic con il pulsante destro del mouse su Active Directory, quindi scegliere Modifica regole attestazione.
3. Nella finestra di dialogo Modifica regole attestazione selezionare la scheda regole di trasformazione accettazione, quindi fare clic su Aggiungi regola per avviare la creazione guidata regola.
4. Nella pagina Seleziona modello di regola, in modello di regola attestazione, selezionare pass-through o filtrare un'attestazione in ingresso dall'elenco, quindi fare clic su Avanti.
5. Nella pagina Configura regola, in nome regola attestazione, digitare il nome visualizzato per la regola. in tipo di attestazione in ingresso digitare l'URL del tipo di attestazione seguente e quindi selezionare passa attraverso tutti i valori di attestazione.</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. Per verificare la regola, selezionarla nell'elenco e fare clic su Modifica regola, quindi su Visualizza lingua regola. Il linguaggio delle regole attestazioni dovrebbe apparire come segue: `c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. Fare clic su fine.
8. Nella finestra di dialogo Modifica regole attestazione fare clic su OK per salvare le regole.
9. Ripetere i passaggi da 2 a 6 per creare una regola attestazioni aggiuntiva per ognuno dei quattro tipi di attestazione rimanenti mostrati sotto fino a quando non sono state create tutte le cinque regole.

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


~~~
`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`
~~~

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>Passaggio 3: aggiornare il relying party trust della piattaforma di identità Microsoft Office 365

Scegliere uno degli scenari di esempio seguenti per configurare le regole attestazioni nella piattaforma di identità Microsoft Office 365 relying party attendibilità che soddisfi meglio le esigenze dell'organizzazione.

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>Scenari dei criteri di accesso client per AD FS 2,0
Nelle sezioni seguenti vengono descritti gli scenari esistenti per AD FS 2,0

### <a name="scenario-1-block-all-external-access-to-office-365"></a>Scenario 1: bloccare tutto l'accesso esterno a Office 365

Questo scenario di criteri di accesso client consente l'accesso da tutti i client interni e blocca tutti i client esterni in base all'indirizzo IP del client esterno. Il set di regole si basa sulla regola di autorizzazione rilascio predefinita consente l'accesso a tutti gli utenti. È possibile utilizzare la procedura seguente per aggiungere una regola di autorizzazione rilascio al trust di Office 365 relying party.

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Per creare una regola per bloccare l'accesso esterno a Office 365



1. Fare clic sul pulsante Start, scegliere programmi, strumenti di amministrazione, quindi fare clic su Gestione AD FS 2,0.
2. Nell'albero della console, in AD FS 2.0 \ relazioni di trust, fare clic su attendibilità del componente, fare clic con il pulsante destro del mouse sull'attendibilità della piattaforma di identità Microsoft Office 365, quindi scegliere Modifica regole attestazione. 
3. Nella finestra di dialogo Modifica regole attestazione selezionare la scheda regole di autorizzazione rilascio e quindi fare clic su Aggiungi regola per avviare la creazione guidata regola attestazione.
4. Nella pagina Seleziona modello di regola, in modello di regola attestazione, selezionare Invia attestazioni mediante una regola personalizzata, quindi fare clic su Avanti.
5. Nella pagina Configura regola, in nome regola attestazione, digitare il nome visualizzato per questa regola. In regola personalizzata digitare o incollare la sintassi del linguaggio delle regole attestazioni seguente: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. Fare clic su fine. Verificare che la nuova regola venga visualizzata immediatamente sotto la regola Consenti accesso a tutti gli utenti nell'elenco regole di autorizzazione rilascio.
7. Per salvare la regola, nella finestra di dialogo Modifica regole attestazione fare clic su OK.

>[!NOTE]
>Sarà necessario sostituire il valore indicato in precedenza per "Public IP address Regex" con un'espressione IP valida. Per ulteriori informazioni, vedere compilazione dell'espressione intervallo di indirizzi IP.


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>Scenario 2: bloccare l'accesso esterno a Office 365 eccetto Exchange ActiveSync

Nell'esempio seguente viene consentito l'accesso a tutte le applicazioni Office 365, incluso Exchange Online, da client interni, incluso Outlook. Blocca l'accesso dai client che si trovano all'esterno della rete aziendale, come indicato dall'indirizzo IP del client, ad eccezione dei client di Exchange ActiveSync, ad esempio smartphone. Il set di regole si basa sulla regola di autorizzazione di rilascio predefinita denominata Consenti l'accesso a tutti gli utenti. Usare la procedura seguente per aggiungere una regola di autorizzazione rilascio al trust di Office 365 relying party usando la creazione guidata regola attestazione:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Per creare una regola per bloccare l'accesso esterno a Office 365



1. Fare clic sul pulsante Start, scegliere programmi, strumenti di amministrazione, quindi fare clic su Gestione AD FS 2,0.
2. Nell'albero della console, in AD FS 2.0 \ relazioni di trust, fare clic su attendibilità del componente, fare clic con il pulsante destro del mouse sull'attendibilità della piattaforma di identità Microsoft Office 365, quindi scegliere Modifica regole attestazione. 
3. Nella finestra di dialogo Modifica regole attestazione selezionare la scheda regole di autorizzazione rilascio e quindi fare clic su Aggiungi regola per avviare la creazione guidata regola attestazione.
4. Nella pagina Seleziona modello di regola, in modello di regola attestazione, selezionare Invia attestazioni mediante una regola personalizzata, quindi fare clic su Avanti.
5. Nella pagina Configura regola, in nome regola attestazione, digitare il nome visualizzato per questa regola. In regola personalizzata digitare o incollare la sintassi del linguaggio delle regole attestazioni seguente: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Fare clic su fine. Verificare che la nuova regola venga visualizzata immediatamente sotto la regola Consenti accesso a tutti gli utenti nell'elenco regole di autorizzazione rilascio.
7. Per salvare la regola, nella finestra di dialogo Modifica regole attestazione fare clic su OK.

>[!NOTE]
>Sarà necessario sostituire il valore indicato in precedenza per "Public IP address Regex" con un'espressione IP valida. Per ulteriori informazioni, vedere compilazione dell'espressione intervallo di indirizzi IP.

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>Scenario 3: bloccare l'accesso esterno a Office 365 ad eccezione delle applicazioni basate su browser

Il set di regole si basa sulla regola di autorizzazione di rilascio predefinita denominata Consenti l'accesso a tutti gli utenti. Usare la procedura seguente per aggiungere una regola di autorizzazione rilascio alla piattaforma di identità Microsoft Office 365 relying party attendibilità usando la creazione guidata regola attestazione:

>[!NOTE]
>Questo scenario non è supportato con un proxy di terze parti a causa di limitazioni sulle intestazioni dei criteri di accesso client con richieste passive (basate sul Web).

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Per creare una regola per bloccare l'accesso esterno a Office 365 ad eccezione delle applicazioni basate su browser



1. Fare clic sul pulsante Start, scegliere programmi, strumenti di amministrazione, quindi fare clic su Gestione AD FS 2,0.
2. Nell'albero della console, in AD FS 2.0 \ relazioni di trust, fare clic su attendibilità del componente, fare clic con il pulsante destro del mouse sull'attendibilità della piattaforma di identità Microsoft Office 365, quindi scegliere Modifica regole attestazione. 
3. Nella finestra di dialogo Modifica regole attestazione selezionare la scheda regole di autorizzazione rilascio e quindi fare clic su Aggiungi regola per avviare la creazione guidata regola attestazione.
4. Nella pagina Seleziona modello di regola, in modello di regola attestazione, selezionare Invia attestazioni mediante una regola personalizzata, quindi fare clic su Avanti.
5. Nella pagina Configura regola, in nome regola attestazione, digitare il nome visualizzato per questa regola. In regola personalizzata digitare o incollare la sintassi del linguaggio delle regole attestazioni seguente: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Fare clic su fine. Verificare che la nuova regola venga visualizzata immediatamente sotto la regola Consenti accesso a tutti gli utenti nell'elenco regole di autorizzazione rilascio.
7. Per salvare la regola, nella finestra di dialogo Modifica regole attestazione fare clic su OK.

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Scenario 4: bloccare tutto l'accesso esterno a Office 365 per gruppi di Active Directory designati

Nell'esempio seguente viene abilitato l'accesso da client interni in base all'indirizzo IP. Blocca l'accesso dai client che si trovano all'esterno della rete aziendale che dispongono di un indirizzo IP client esterno, ad eccezione di quelli inclusi in un gruppo di Active Directory specificato. il set di regole si basa sulla regola di autorizzazione di rilascio predefinita denominata Consenti accesso a Tutti gli utenti. Usare la procedura seguente per aggiungere una regola di autorizzazione rilascio alla piattaforma di identità Microsoft Office 365 relying party attendibilità usando la creazione guidata regola attestazione:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Per creare una regola per bloccare l'accesso esterno a Office 365 per i gruppi di Active Directory designati



1. Fare clic sul pulsante Start, scegliere programmi, strumenti di amministrazione, quindi fare clic su Gestione AD FS 2,0.
2. Nell'albero della console, in AD FS 2.0 \ relazioni di trust, fare clic su attendibilità del componente, fare clic con il pulsante destro del mouse sull'attendibilità della piattaforma di identità Microsoft Office 365, quindi scegliere Modifica regole attestazione. 
3. Nella finestra di dialogo Modifica regole attestazione selezionare la scheda regole di autorizzazione rilascio e quindi fare clic su Aggiungi regola per avviare la creazione guidata regola attestazione.
4. Nella pagina Seleziona modello di regola, in modello di regola attestazione, selezionare Invia attestazioni mediante una regola personalizzata, quindi fare clic su Avanti.
5. Nella pagina Configura regola, in nome regola attestazione, digitare il nome visualizzato per questa regola. In regola personalizzata digitare o incollare la sintassi del linguaggio delle regole attestazioni seguente: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Fare clic su fine. Verificare che la nuova regola venga visualizzata immediatamente sotto la regola Consenti accesso a tutti gli utenti nell'elenco regole di autorizzazione rilascio.
7. Per salvare la regola, nella finestra di dialogo Modifica regole attestazione fare clic su OK.


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>Descrizioni della sintassi del linguaggio delle regole attestazioni usata negli scenari precedenti

|                                                                                                   Descrizione                                                                                                   |                                                                     Sintassi del linguaggio delle regole attestazioni                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              AD FS regola predefinita per consentire l'accesso a tutti gli utenti. Questa regola deve essere già presente nell'elenco delle regole di autorizzazione di rilascio di Microsoft Office 365 relying party trust.              |                                  = > problema (Type = "<https://schemas.microsoft.com/authorization/claims/permit>", valore = "true");                                   |
|                               L'aggiunta di questa clausola a una nuova regola personalizzata specifica che la richiesta proviene dal proxy server federativo (ovvero ha l'intestazione x-MS-Proxy)                                |                                                                                                                                                                    |
|                                                                                 È consigliabile che tutte le regole includano questo.                                                                                  |                                    Exists ([tipo = = "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy>"])                                    |
|                                                         Usato per stabilire che la richiesta provenga da un client con un indirizzo IP compreso nell'intervallo consentito definito.                                                         | NOT EXISTS ([tipo = = "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip>", valore = ~ "Regex indirizzo IP pubblico fornito dal cliente"]) |
|                                    Questa clausola viene utilizzata per specificare che se l'applicazione a cui si accede non è Microsoft. Exchange. ActiveSync la richiesta deve essere negata.                                     |       NOT EXISTS ([Type = = "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application>", valore = = "Microsoft. Exchange. ActiveSync"])        |
|                                                      Questa regola consente di determinare se la chiamata è stata tramite un Web browser e non verrà negata.                                                      |              NOT EXISTS ([Type = = "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path>", valore = = "/adfs/ls/"])               |
| Questa regola indica che gli unici utenti in un particolare gruppo di Active Directory (in base al valore SID) devono essere negati. L'aggiunta di NOT a questa istruzione indica che un gruppo di utenti sarà consentito, indipendentemente dalla posizione. |             Exists ([tipo = = "<https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid>", valore = ~ "{valore SID del gruppo di Active Directory consentito}"])              |
|                                                                Si tratta di una clausola obbligatoria per emettere un'istruzione Deny quando vengono soddisfatte tutte le condizioni precedenti.                                                                 |                                   = > problema (Type = "<https://schemas.microsoft.com/authorization/claims/deny>", valore = "true");                                    |

### <a name="building-the-ip-address-range-expression"></a>Compilazione dell'espressione intervallo di indirizzi IP

L'attestazione x-ms-inoltred-client-IP viene popolata da un'intestazione HTTP attualmente impostata solo da Exchange Online, che popola l'intestazione quando passa la richiesta di autenticazione a AD FS. Il valore dell'attestazione può essere uno dei seguenti:

>[!Note] 
>Exchange Online supporta attualmente solo indirizzi IPV4 e non IPV6.

Un singolo indirizzo IP: l'indirizzo IP del client connesso direttamente a Exchange Online

>[!Note] 
>L'indirizzo IP di un client nella rete aziendale verrà visualizzato come indirizzo IP dell'interfaccia esterna del proxy o del gateway dell'organizzazione in uscita.

I client connessi alla rete aziendale da una VPN o da Microsoft DirectAccess (DA) possono essere visualizzati come client aziendali interni o come client esterni, a seconda della configurazione di VPN o DA.

Uno o più indirizzi IP: quando Exchange Online non è in grado di determinare l'indirizzo IP del client che si connette, il valore verrà impostato in base al valore dell'intestazione x-inoltred-for, un'intestazione non standard che può essere inclusa nelle richieste basate su HTTP ed è supportata da molti client, bilanciamenti del carico e proxy sul mercato.

>[!Note]
>Più indirizzi IP, che indicano l'indirizzo IP del client e l'indirizzo di ogni proxy che ha superato la richiesta, saranno separati da una virgola.

Gli indirizzi IP correlati all'infrastruttura di Exchange Online non verranno visualizzati nell'elenco.


#### <a name="regular-expressions"></a>Espressioni regolari

Quando è necessario trovare una corrispondenza per un intervallo di indirizzi IP, è necessario creare un'espressione regolare per eseguire il confronto. Nella serie successiva di passaggi, si forniranno esempi per la creazione di un'espressione di questo tipo in modo che corrispondano agli intervalli di indirizzi seguenti (si noti che sarà necessario modificare questi esempi in modo che corrispondano all'intervallo di indirizzi IP pubblici):


- 192.168.1.1 – 192.168.1.25
- 10.0.0.1-10.0.0.14

In primo luogo, il modello di base che corrisponderà a un singolo indirizzo IP è il seguente: \b # # #\.###\.###\.# # # \b

Estendendo questo, possiamo trovare la corrispondenza con due indirizzi IP diversi con un'espressione OR come segue: \b # # #\.###\.###\.# # # \b | \b # # #\.###\.###\.# # # \b

Un esempio per trovare una corrispondenza con due soli indirizzi (ad esempio 192.168.1.1 o 10.0.0.1) sarà: \b192\.168\.1\.1 \ b | \b10\.0\.0\.1 \ b

In questo modo si ottiene la tecnica con cui è possibile immettere un numero qualsiasi di indirizzi. Se è necessario consentire un intervallo di indirizzi, ad esempio 192.168.1.1-192.168.1.25, la corrispondenza deve essere eseguita carattere per carattere: \b192\.168\.1\.([1-9] | 1 [0-9] | 2 [0-5]) \b

>[!Note] 
>L'indirizzo IP viene considerato come stringa e non come numero.


La regola viene suddivisa nel modo seguente: \b192\.168\.1\.

Corrisponde a qualsiasi valore che inizia con 192.168.1.

Il codice seguente corrisponde agli intervalli necessari per la parte dell'indirizzo dopo il separatore decimale finale:


- ([1-9] corrisponde a indirizzi che terminano con 1-9
- | 1 [0-9] corrisponde a indirizzi che terminano con 10-19
- | 2 [0-5]) corrisponde a indirizzi che terminano con 20-25

>[!Note]
>Le parentesi devono essere posizionate correttamente, in modo che non venga avviata la corrispondenza di altre parti di indirizzi IP.

Con la corrispondenza del blocco 192, è possibile scrivere un'espressione simile per il blocco 10: \b10\.0\.0\.([1-9] | 1 [0-4]) \b

E mettendoli insieme, l'espressione seguente deve corrispondere a tutti gli indirizzi per "192.168.1.1 ~ 25" e "10.0.0.1 ~ 14": \b192\.168\.1\.([1-9] | 1 [0-9] | 2 [0-5]) \b | \b10\.0\.0\.([1-9] | 1 [0-4]) \b

#### <a name="testing-the-expression"></a>Test dell'espressione

Le espressioni Regex possono diventare piuttosto complesse, quindi è consigliabile usare uno strumento di verifica Regex. Se si esegue una ricerca in Internet per "generatore di espressioni Regex online", sono disponibili diverse utilità online utili che consentono di provare le espressioni sui dati di esempio.

Quando si esegue il test dell'espressione, è importante comprendere cosa si prevede di dover trovare la corrispondenza. Il sistema Exchange Online può inviare molti indirizzi IP separati da virgole. Le espressioni fornite sopra funzioneranno per questa operazione. Tuttavia, è importante considerare questo aspetto quando si testano le espressioni Regex. Ad esempio, è possibile usare l'input di esempio seguente per verificare gli esempi precedenti: 

192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20 

10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10, 0.0.1 











## <a name="validating-the-deployment"></a>Convalida della distribuzione

### <a name="security-audit-logs"></a>Log di controllo di sicurezza

Per verificare che le nuove attestazioni del contesto della richiesta vengano inviate e siano disponibili per la pipeline di elaborazione delle attestazioni AD FS, abilitare la registrazione di controllo nel server di AD FS. Inviare quindi alcune richieste di autenticazione e verificare i valori di attestazione nelle voci del registro di controllo di sicurezza standard. 

Per abilitare la registrazione degli eventi di controllo nel registro di sicurezza su un server AD FS, seguire i passaggi descritti in configurare il controllo per AD FS 2,0.

### <a name="event-logging"></a>Registrazione degli eventi

Per impostazione predefinita, le richieste non riuscite vengono registrate nel registro eventi dell'applicazione che si trova in registri applicazioni e servizi \ AD FS 2,0 \ amministratore. per ulteriori informazioni sulla registrazione eventi per AD FS, vedere [configurare 2,0 ad FS registrazione eventi](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>Configurazione dei log di traccia dettagliati AD FS

AD FS eventi di traccia vengono registrati nel log di debug AD FS 2,0. Per abilitare la traccia, vedere [configurare la traccia di debug per AD FS 2,0](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

Dopo aver abilitato la traccia, usare la sintassi della riga di comando seguente per abilitare il livello di registrazione dettagliato: wevtutil. exe SL "AD FS 2,0 Tracing/debug"/l: 5  

## <a name="related"></a>Informazioni correlate
Per ulteriori informazioni sui nuovi tipi di attestazione, vedere [ad FS tipi di attestazione](AD-FS-Claims-Types.md).

