---
ms.assetid: 
title: Criteri di controllo di accesso client in Active Directory Federation Services 2.0
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 00b9cd17c7a5c206e06bea12fd90762adb336715
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>Criteri di controllo di accesso client in AD FS 2.0
Un criteri di accesso client in Active Directory Federation Services 2.0 consentono di limitare o concedere agli utenti di accedere alle risorse.  Questo documento viene descritto come abilitare i criteri di accesso client in AD FS 2.0 e come configurare gli scenari più comuni.

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>Abilitare il criterio di accesso Client in AD FS 2.0

Per abilitare i criteri di accesso client, attenersi alla procedura seguente.

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>Passaggio 1: Installare l'aggiornamento cumulativo 2 per AD FS 2.0 pacchetto nei server AD FS

Scaricare il [aggiornamento cumulativo 2 per Active Directory Federation Services (ADFS) 2.0](https://support.microsoft.com/en-us/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0) pacchetto e installarlo in tutti i server federativi e proxy server federativi.

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>Passaggio 2: Aggiungere che cinque attestazione regole all'attendibilità del Provider di attestazioni di Active Directory

Dopo l'installazione dell'aggiornamento cumulativo 2 in tutti i server ADFS e proxy, è possibile utilizzare la procedura seguente per aggiungere un set di regole attestazioni che rende disponibili per il motore dei criteri i nuovi tipi di attestazione.

A tale scopo, saranno aggiunti cinque regole di trasformazione accettazione per ognuno dei tipi di attestazione contesto richiesta nuovo usando la procedura seguente.

Nell'attendibilità provider di attestazioni di Active Directory, creare una nuova regola di trasformazione accettazione per pass-through ognuno dei nuovi tipi di attestazione contesto richiesta.

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>Per aggiungere una regola attestazione in Active Directory trust del provider di attestazioni per ogni del contesto dei cinque tipi di attestazione:


1. Fare clic su Start, scegliere i programmi, strumenti di amministrazione e quindi fare clic su AD FS 2.0 gestione.
2. Nell'albero della console, in AD FS 2.0 \ relazioni, fare clic su Provider di attestazioni, fare doppio clic su Active Directory e quindi fare clic su Modifica regole attestazione.
3. Nella finestra di dialogo Modifica regole attestazione, selezionare la scheda regole di trasformazione accettazione e quindi fare clic su Aggiungi regola per avviare la creazione guidata regola.
4. Nella pagina Seleziona modello di regola, in modello di regola attestazione, selezionare Pass-Through o filtrare un'attestazione in ingresso dall'elenco e quindi fare clic su Avanti.
5. Nella pagina Configura regola, in nome regola attestazione, digitare il nome visualizzato per questa regola. in ingresso tipo di attestazione, digitare l'URL di tipo di attestazione seguenti e quindi seleziona Pass tramite tutti i valori attestazione.</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. Per verificare la regola, selezionala nell'elenco e fare clic su Modifica regola, quindi fare clic su Visualizza lingua delle regole. Il linguaggio di regola attestazione dovrebbe essere visualizzato come segue: `c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. Fare clic su Fine.
8. Nella finestra di dialogo Modifica regole attestazione, fare clic su OK per salvare le regole.
9. Ripetere i passaggi da 2 a 6 per creare una regola attestazione aggiuntive per ognuno dei tipi di quattro attestazione rimanenti illustrati di seguito fino a quando non sono state create tutte le regole di cinque.

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>Passaggio 3: Aggiornare la piattaforma delle identità Microsoft Office 365 trust della relying party

Scegliere uno degli scenari di esempio seguenti per configurare le regole di attestazione nella piattaforma Microsoft Office 365 identità trust della relying party che meglio soddisfi le esigenze dell'organizzazione.

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>Scenari di criteri di accesso client per AD FS 2.0
Le sezioni seguenti verranno descritto gli scenari che esiste per AD FS 2.0

### <a name="scenario-1-block-all-external-access-to-office-365"></a>Scenario 1: Bloccare completamente l'accesso esterno a Office 365

Questo scenario di criteri di accesso client consente l'accesso da tutti i client interni e i blocchi di tutti i client esterni in base all'indirizzo IP del client esterni. Il set di regole si basa sulla regola di autorizzazione rilascio predefinita Consenti accesso a tutti gli utenti. È possibile utilizzare la procedura seguente per aggiungere una regola di autorizzazione di rilascio per il trust della relying party di Office 365.

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Per creare una regola per bloccare completamente l'accesso esterno a Office 365



1. Fare clic su Start, scegliere i programmi, strumenti di amministrazione e quindi fare clic su AD FS 2.0 gestione.
2. Nell'albero della console, in AD FS 2.0 \ relazioni, fare clic su attendibilità, fare doppio clic su trust della piattaforma delle identità di Microsoft Office 365 e quindi fare clic su Modifica regole attestazione. 
3. Nella finestra di dialogo Modifica regole attestazione, selezionare la scheda regole di autorizzazione rilascio e quindi fare clic su Aggiungi regola per avviare la creazione guidata regola di attestazione.
4. Nella pagina Seleziona modello di regola, in modello di regola attestazione, selezionare inviare attestazioni mediante una regola personalizzata e quindi fare clic su Avanti.
5. Nella pagina Configura regola, in nome regola attestazione, digitare il nome visualizzato per questa regola. In regola personalizzata, digitare o incollare la sintassi del linguaggio di regola attestazione seguenti: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. Fare clic su Fine. Verificare che la nuova regola viene immediatamente visualizzato sotto l'accesso consentire alla regola di tutti gli utenti nell'elenco delle regole di autorizzazione rilascio.
7. Per salvare la regola, nella finestra di dialogo Modifica regole attestazione, fare clic su OK.

>[!NOTE]
>È necessario sostituire il valore di sopra per "regex indirizzo ip pubblico" con un'espressione di IP valida. vedere la compilazione dell'espressione intervallo di indirizzi IP per ulteriori informazioni.


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>Scenario 2: Bloccare completamente l'accesso esterno a Office 365, ad eccezione di Exchange ActiveSync

L'esempio seguente consente di accedere a tutte le applicazioni di Office 365, tra cui Exchange Online, provenienti dai client interni inclusi Outlook. Consente di bloccare l'accesso da client che si trovano all'esterno della rete azienda, come indicato dall'indirizzo IP del client, fatta eccezione per i client di Exchange ActiveSync, ad esempio Smartphone. Il set di regole si basa sulla regola di autorizzazione di rilascio predefinito denominata Consenti accesso a tutti gli utenti. Utilizzare la procedura seguente per aggiungere una regola di autorizzazione di rilascio a Office 365 trust della relying party utilizzando la creazione guidata regola di attestazione:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Per creare una regola per bloccare completamente l'accesso esterno a Office 365



1. Fare clic su Start, scegliere i programmi, strumenti di amministrazione e quindi fare clic su AD FS 2.0 gestione.
2. Nell'albero della console, in AD FS 2.0 \ relazioni, fare clic su attendibilità, fare doppio clic su trust della piattaforma delle identità di Microsoft Office 365 e quindi fare clic su Modifica regole attestazione. 
3. Nella finestra di dialogo Modifica regole attestazione, selezionare la scheda regole di autorizzazione rilascio e quindi fare clic su Aggiungi regola per avviare la creazione guidata regola di attestazione.
4. Nella pagina Seleziona modello di regola, in modello di regola attestazione, selezionare inviare attestazioni mediante una regola personalizzata e quindi fare clic su Avanti.
5. Nella pagina Configura regola, in nome regola attestazione, digitare il nome visualizzato per questa regola. In regola personalizzata, digitare o incollare la sintassi del linguaggio di regola attestazione seguenti: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Fare clic su Fine. Verificare che la nuova regola viene immediatamente visualizzato sotto l'accesso consentire alla regola di tutti gli utenti nell'elenco delle regole di autorizzazione rilascio.
7. Per salvare la regola, nella finestra di dialogo Modifica regole attestazione, fare clic su OK.

>[!NOTE]
>È necessario sostituire il valore di sopra per "regex indirizzo ip pubblico" con un'espressione di IP valida. vedere la compilazione dell'espressione intervallo di indirizzi IP per ulteriori informazioni.

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>Scenario 3: Bloccare completamente l'accesso esterno a Office 365, ad eccezione di applicazioni basate su browser

Il set di regole si basa sulla regola di autorizzazione di rilascio predefinito denominata Consenti accesso a tutti gli utenti. Utilizzare la procedura seguente per aggiungere una regola di autorizzazione di rilascio per la piattaforma delle identità Microsoft Office 365 trust della relying party utilizzando la creazione guidata regola di attestazione:

>[!NOTE]
>Questo scenario non è supportato con un proxy di terze parti a causa delle limitazioni sulle intestazioni di criteri di accesso client con le richieste di passive (basata sul Web).

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Per creare una regola per bloccare completamente l'accesso esterno a Office 365, ad eccezione di applicazioni basate su browser



1. Fare clic su Start, scegliere i programmi, strumenti di amministrazione e quindi fare clic su AD FS 2.0 gestione.
2. Nell'albero della console, in AD FS 2.0 \ relazioni, fare clic su attendibilità, fare doppio clic su trust della piattaforma delle identità di Microsoft Office 365 e quindi fare clic su Modifica regole attestazione. 
3. Nella finestra di dialogo Modifica regole attestazione, selezionare la scheda regole di autorizzazione rilascio e quindi fare clic su Aggiungi regola per avviare la creazione guidata regola di attestazione.
4. Nella pagina Seleziona modello di regola, in modello di regola attestazione, selezionare inviare attestazioni mediante una regola personalizzata e quindi fare clic su Avanti.
5. Nella pagina Configura regola, in nome regola attestazione, digitare il nome visualizzato per questa regola. In regola personalizzata, digitare o incollare la sintassi del linguaggio di regola attestazione seguenti: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Fare clic su Fine. Verificare che la nuova regola viene immediatamente visualizzato sotto l'accesso consentire alla regola di tutti gli utenti nell'elenco delle regole di autorizzazione rilascio.
7. Per salvare la regola, nella finestra di dialogo Modifica regole attestazione, fare clic su OK.

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Scenario 4: Bloccare completamente l'accesso esterno a Office 365 per gruppi specifici di Active Directory

L'esempio seguente consente l'accesso dai client interni basate sull'indirizzo IP. Viene bloccato l'accesso da client che si trovano all'esterno della rete azienda con un indirizzo IP client esterni, fatta eccezione per gli utenti in una regola. l'argomento di Active Directory specificato set estende la regola di autorizzazione di rilascio predefinito denominata Consenti accesso a tutti gli utenti. Utilizzare la procedura seguente per aggiungere una regola di autorizzazione di rilascio per la piattaforma delle identità Microsoft Office 365 trust della relying party utilizzando la creazione guidata regola di attestazione:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Per creare una regola per bloccare completamente l'accesso esterno a Office 365 per gruppi specifici di Active Directory



1. Fare clic su Start, scegliere i programmi, strumenti di amministrazione e quindi fare clic su AD FS 2.0 gestione.
2. Nell'albero della console, in AD FS 2.0 \ relazioni, fare clic su attendibilità, fare doppio clic su trust della piattaforma delle identità di Microsoft Office 365 e quindi fare clic su Modifica regole attestazione. 
3. Nella finestra di dialogo Modifica regole attestazione, selezionare la scheda regole di autorizzazione rilascio e quindi fare clic su Aggiungi regola per avviare la creazione guidata regola di attestazione.
4. Nella pagina Seleziona modello di regola, in modello di regola attestazione, selezionare inviare attestazioni mediante una regola personalizzata e quindi fare clic su Avanti.
5. Nella pagina Configura regola, in nome regola attestazione, digitare il nome visualizzato per questa regola. In regola personalizzata, digitare o incollare la sintassi del linguaggio di regola attestazione seguenti: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Fare clic su Fine. Verificare che la nuova regola viene immediatamente visualizzato sotto l'accesso consentire alla regola di tutti gli utenti nell'elenco delle regole di autorizzazione rilascio.
7. Per salvare la regola, nella finestra di dialogo Modifica regole attestazione, fare clic su OK.


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>Descrizioni della sintassi di linguaggio di regola attestazione usato negli scenari precedenti

|Descrizione|Sintassi del linguaggio di regola attestazione|
|-----|-----| 
|Regola di predefinita AD FS per consentire l'accesso a tutti gli utenti. Questa regola deve esistere già nel trust della relying party elenco di regole di autorizzazione rilascio della piattaforma di identità di Microsoft Office 365.|= > problema (tipo = "https://schemas.microsoft.com/authorization/claims/permit", valore = "true");| 
|Aggiunta di questa clausola a una regola personalizzata specifica che la richiesta provenga da proxy server federativo (ad esempio, con l'intestazione x-ms-proxy)
È consigliabile che tutte le regole di includono questa operazione.|Esiste ([tipo = = "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"])| 
|Consente di stabilire che la richiesta proviene da un client con un indirizzo IP nell'intervallo accettabile definito.|NON esiste ([tipo = = "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", valore = ~ "regex indirizzo ip pubblico forniti dal cliente"])| 
|Questa clausola viene utilizzata per specificare che se l'applicazione di cui si accede non è Microsoft.Exchange.ActiveSync deve essere rifiutata la richiesta.|NON esiste ([tipo = = "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value=="Microsoft.Exchange.ActiveSync"])| 
|Questa regola consente di determinare se la chiamata è stata tramite un Web browser e non verrà negata.|NON esiste ([tipo = = "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", valore = = "/ adfs/ls /"])| 
|Questa regola indica che gli utenti soli in un determinato gruppo di Active Directory (basato su valore SID) devono essere negati. Aggiunta di questa informativa non significa che un gruppo di utenti sarà consentito, indipendentemente dalla posizione.|Esiste ([tipo = = "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", valore = ~ "{gruppo valore del SID del gruppo di Active Directory consentita}"])| 
|Si tratta di una clausola necessaria per emettere una Nega quando vengono soddisfatte tutte le condizioni precedenti.|= > problema (tipo = "https://schemas.microsoft.com/authorization/claims/deny", valore = "true");|

### <a name="building-the-ip-address-range-expression"></a>La compilazione dell'espressione intervallo di indirizzi IP

L'attestazione x-ms-inoltrati-client-ip viene compilato da un'intestazione HTTP attualmente impostata solo da Exchange Online, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione per ADFS. Il valore dell'attestazione può essere uno dei seguenti:

>[!Note] 
>Exchange Online supporta attualmente solo gli indirizzi IPV4 e IPV6 non.

Un singolo indirizzo IP: indirizzo IP del client connessi direttamente a Exchange Online

>[!Note] 
>L'indirizzo IP di un client nella rete aziendale verrà visualizzato come l'indirizzo IP di interfaccia esterna del proxy in uscita dell'organizzazione o un gateway.

I client connessi alla rete aziendale tramite una VPN o DirectAccess da Microsoft (DA) potrebbero apparire come client aziendale interno o come i client esterni a seconda della configurazione di VPN o DA.

Uno o più indirizzi IP: quando Exchange Online non può determinare l'indirizzo IP del client, imposterà il valore in base al valore dell'intestazione x-inoltrati-per, un'intestazione non standard che può essere inclusi in richieste basate su HTTP e supportata da molti client, i servizi di bilanciamento del carico e proxy sul mercato.

>[!Note]
>Più indirizzi IP, che indica l'indirizzo IP del client e l'indirizzo di ciascun proxy che ha trasmesso la richiesta, saranno separati da una virgola.

Gli indirizzi IP relativi all'infrastruttura Exchange Online non verranno visualizzati nell'elenco.


#### <a name="regular-expressions"></a>Espressioni regolari

Quando si deve corrispondere a un intervallo di indirizzi IP, si rende necessario costruire un'espressione regolare per eseguire il confronto. Nella successiva serie di passaggi, verranno forniti esempi su come costruire un'espressione di questo tipo per soddisfare gli intervalli di indirizzi seguenti (si noti che è necessario modificare questi esempi per l'intervallo di IP pubblico corrisponde):


- 192.168.1.1 – 192.168.1.25
- 10.0.0.1 – 10.0.0.14

Prima di tutto, il modello di base corrispondente a un singolo indirizzo IP è il seguente: \b###\.###\.###\.###\b

Questa estensione, abbiamo possiamo corrispondano come segue due indirizzi IP diversi con un'espressione o: \b###\.###\.###\.###\b|\b###\.###\.###\.###\b

Un esempio per trovare la corrispondenza solo due indirizzi (ad esempio 192.168.1.1 o 10.0.0.1), potrebbe essere: \b192\.168\.1\.1\b|\b10\.0\.0\.1\b

In questo modo la tecnica mediante il quale è possibile immettere qualsiasi numero di indirizzi. Dove necessario consentite, ad esempio 192.168.1.1: 192.168.1.25, un intervallo di indirizzi corrispondente operazione deve essere eseguito carattere dal carattere: \b192\.168\.1\. ([1-9] | 1 [0-9] & 2 [0-5]) \b

>[!Note] 
>L'indirizzo IP viene considerato come stringa e non un numero.


La regola è suddiviso come segue: \b192\.168\.1\.

Questa corrisponde a partire qualsiasi valore da 192.168.1.

Gli intervalli necessari per la parte dell'indirizzo dopo il punto decimale finale corrisponde a quanto segue:


- ([1-9] corrispondenze indirizzi che terminano con 1-9
- | 1 [0-9] corrisponde a indirizzi che terminano con 10-19
- Indirizzi di corrispondenze |2[0-5]) che termina con 20-25

>[!Note]
>Le parentesi devono essere posizionate correttamente, in modo che non verrà avviato corrispondenza altre parti di indirizzi IP.

Con il blocco di 192 corrispondente, è possibile scrivere un'espressione simile per il blocco di 10: \b10\.0\.0\. ([1-9] | 1 [0-4]) \b

E inserirli insieme, l'espressione seguente deve corrispondere tutti gli indirizzi per "192.168.1.1~25" e "10.0.0.1~14": \b192\.168\.1\. ([1-9]|1[0-9]|2[0-5])\b|\b10\.0\.0\. ([1-9] | 1 [0-4]) \b

#### <a name="testing-the-expression"></a>L'espressione di test

Espressioni regolari espressioni possono diventare molto complessa, quindi ti consigliamo vivamente di uno strumento di verifica delle espressioni regolari. Se si esegue una ricerca in internet per "generatore di espressioni regex online", sono disponibili diverse utilità online buona che consente di provare le espressioni in base a dati di esempio.

Quando l'espressione di test, è importante comprendere che cosa prevede che siano disponibili trovare la corrispondenza. Il sistema di Exchange può inviare molti gli indirizzi IP, separati da virgole. Le espressioni fornite in precedenza funzionerà per questo. Tuttavia, è importante affrontare questa durante il test delle espressioni di espressioni regolari. Ad esempio, una possibile utilizzare il seguente codice di esempio di input per verificare gli esempi precedenti: 

192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20 

10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1 











## <a name="validating-the-deployment"></a>Convalida della distribuzione

### <a name="security-audit-logs"></a>Registri di controllo di sicurezza

Per verificare che il nuovo contesto richiesta attestazioni vengono inviate e sono disponibili per AD FS attestazioni di pipeline di elaborazione, abilitare il controllo sul server ADFS. Quindi inviare alcune richieste di autenticazione e controllo per i valori di attestazione nelle voci del Registro di controllo standard di sicurezza. 

Per abilitare la registrazione di controllo del registro degli eventi per la sicurezza in un server ADFS, seguire i passaggi nella configurazione di controllo per ADFS 2.0.

### <a name="event-logging"></a>Registrazione degli eventi

Per impostazione predefinita, le richieste non riuscite vengono registrate nel registro eventi dell'applicazione si trova in registri applicazioni e servizi \ AD FS 2.0 \ Admin.For ulteriori informazioni sulla registrazione degli eventi per ADFS, vedere [configurare Active Directory di registrazione degli eventi ADFS 2.0](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>Configurazione di ADFS Verbose nel log di traccia

Gli eventi di traccia di AD FS vengono registrati nel Registro di AD FS 2.0 debug. Per abilitare la traccia, vedere [Configura traccia di debug per AD FS 2.0](https://technet.microsoft.com/en-us/library/adfs2-troubleshooting-configuring-computers.aspx).

Dopo avere abilitato la traccia, usare la sintassi della riga di comando seguente per abilitare il livello di registrazione dettagliata: wevtutil.exe sl "AD FS 2.0 traccia/Debug" /l:5  

## <a name="related"></a>Correlati
Per ulteriori informazioni su nuovi tipi di attestazione vedere [tipi di attestazioni di AD FS](AD-FS-Claims-Types.md).
 
