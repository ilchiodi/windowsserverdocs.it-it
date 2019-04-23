---
ms.assetid: ''
title: Criteri di controllo di accesso client in Active Directory Federation Services 2.0
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 216af933aee643ee56feff71c59d9ecc2e62998c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842992"
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>Criteri di controllo di accesso client in AD FS 2.0
Un criteri di accesso client in Active Directory Federation Services 2.0 consentono di limitare o concedere agli utenti di accedere alle risorse.  Questo documento descrive come abilitare i criteri di accesso client in AD FS 2.0 e come configurare gli scenari più comuni.

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>Abilitazione di criteri di accesso Client in AD FS 2.0

Per abilitare i criteri di accesso client, attenersi alla procedura seguente.

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>Passaggio 1: Installare l'aggiornamento cumulativo 2 per AD FS 2.0 del pacchetto nel server AD FS

Scaricare il [aggiornamento cumulativo 2 per Active Directory Federation Services (ADFS) 2.0](https://support.microsoft.com/en-us/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0) del pacchetto e installarlo in tutti i server federativi e proxy server federativo.

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>Passaggio 2: Aggiungere che cinque attestazione regole all'attendibilità del Provider di attestazioni Active Directory

Dopo l'installazione dell'aggiornamento cumulativo 2 su tutti i server AD FS e proxy, utilizzare la procedura seguente per aggiungere un set di regole attestazioni che rende i nuovi tipi di attestazione disponibili per il motore dei criteri.

A tale scopo, si aggiungerà cinque regole di trasformazione accettazione per ogni nuovi tipi di richiesta rapida attestazione usando la procedura seguente.

Nell'attendibilità del provider di attestazioni di Active Directory, creare una nuova regola di trasformazione accettazione per pass-through ognuno dei nuovi tipi di attestazione contesto richiesta.

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>Per aggiungere una regola attestazione ad Active Directory trust del provider di attestazioni per ogni contesto di cinque tipi di attestazione:


1. Fare clic su Start, scegliere Programmi, strumenti di amministrazione e quindi fare clic su AD FS 2.0 Management.
2. Nell'albero della console, in AD FS 2.0\Trust Relationships, fare clic su Provider di attestazioni e quindi fare clic su Edit Claim Rules fare doppio clic su Active Directory.
3. Nella finestra di dialogo Modifica regole attestazione, selezionare la scheda regole di trasformazione accettazione e quindi fare clic su Aggiungi regola per avviare la creazione guidata regola.
4. Nella pagina Seleziona modello di regola, in modello di regola attestazione, selezionare Pass-Through o filtrare un'attestazione in ingresso dall'elenco e quindi fare clic su Avanti.
5. Nella pagina Configura regola sotto Nome regola attestazione, digitare il nome visualizzato per questa regola. in ingresso tipo di attestazione, digitare l'URL di tipo di attestazione seguenti e quindi passare tutti i valori di attestazione.</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. Per verificare la regola, selezionarlo nell'elenco e fare clic su Modifica regola e quindi fare clic su Visualizza lingua delle regole. Il linguaggio di regola attestazione apparirà come segue: `c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. Fare clic su Fine.
8. Nella finestra di dialogo Modifica regole attestazione, fare clic su OK per salvare le regole.
9. Ripetere i passaggi da 2 a 6 per creare una regola attestazioni aggiuntive per ognuno dei tipi di quattro attestazione rimanenti illustrati di seguito fino a quando non sono state create tutte le regole di cinque.

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>Passaggio 3: Aggiornare la piattaforma delle identità Microsoft Office 365 trust della relying party

Scegliere uno degli scenari di esempio riportato di seguito per configurare le regole delle attestazioni nella piattaforma Microsoft Office 365 identità trust della relying party che meglio soddisfa le esigenze della propria organizzazione.

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>Scenari relativi ai criteri di accesso client per AD FS 2.0
Le sezioni seguenti descrivono gli scenari esistenti per AD FS 2.0

### <a name="scenario-1-block-all-external-access-to-office-365"></a>Scenario 1: Bloccare completamente l'accesso esterno a Office 365

Questo scenario di criteri di accesso client consente l'accesso da tutti i client interni e blocchi di tutti i client esterni in base all'indirizzo IP del client esterni. La regola di autorizzazione rilascio predefiniti Consenti accesso a tutti gli utenti si basa il set di regole. È possibile utilizzare la procedura seguente per aggiungere una regola di autorizzazione di rilascio per il trust della relying party di Office 365.

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Per creare una regola per bloccare tutto l'accesso esterno a Office 365



1. Fare clic su Start, scegliere Programmi, strumenti di amministrazione e quindi fare clic su AD FS 2.0 Management.
2. Nell'albero della console, in AD FS 2.0\Trust Relationships, fare clic su trust Relying Party e quindi fare clic su Edit Claim Rules fare doppio clic su trust della piattaforma delle identità di Microsoft Office 365. 
3. Nella finestra di dialogo Modifica regole attestazione, selezionare la scheda regole di autorizzazione rilascio e quindi fare clic su Aggiungi regola per avviare la creazione guidata regola di attestazione.
4. Nella pagina Seleziona modello di regola, in modello di regola attestazione, selezionare inviare attestazioni mediante una regola personalizzata e quindi fare clic su Avanti.
5. Nella pagina Configura regola sotto Nome regola attestazione, digitare il nome visualizzato per questa regola. In regola personalizzata, digitare o incollare la seguente sintassi del linguaggio di regola attestazione: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. Fare clic su Fine. Verificare che la nuova regola venga visualizzato immediatamente sotto il Consenti accesso a regole di tutti gli utenti nell'elenco delle regole di autorizzazione di rilascio.
7. Per salvare la regola, nella finestra di dialogo Modifica regole attestazione, fare clic su OK.

>[!NOTE]
>Dovrai sostituire il valore di sopra di "espressione regolare indirizzo ip pubblico" con un'espressione di IP valida. vedere la compilazione dell'espressione di intervallo di indirizzi IP per altre informazioni.


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>Scenario 2: Bloccare completamente l'accesso esterno a Office 365, ad eccezione di Exchange ActiveSync

Nell'esempio seguente consente di accedere a tutte le applicazioni di Office 365, inclusi Exchange Online, provenienti dai client interni tra cui Outlook. Consente di bloccare l'accesso dai client che si trovano all'esterno della rete azienda, come indicato dall'indirizzo IP del client, eccetto il client di Exchange ActiveSync, ad esempio Smartphone. Il set di regole si basa sulla regola di autorizzazione rilascio predefinita denominata Consenti accesso a tutti gli utenti. Usare la procedura seguente per aggiungere una regola di autorizzazione di rilascio per il trust della relying party usando la procedura guidata regole attestazione di Office 365:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Per creare una regola per bloccare tutto l'accesso esterno a Office 365



1. Fare clic su Start, scegliere Programmi, strumenti di amministrazione e quindi fare clic su AD FS 2.0 Management.
2. Nell'albero della console, in AD FS 2.0\Trust Relationships, fare clic su trust Relying Party e quindi fare clic su Edit Claim Rules fare doppio clic su trust della piattaforma delle identità di Microsoft Office 365. 
3. Nella finestra di dialogo Modifica regole attestazione, selezionare la scheda regole di autorizzazione rilascio e quindi fare clic su Aggiungi regola per avviare la creazione guidata regola di attestazione.
4. Nella pagina Seleziona modello di regola, in modello di regola attestazione, selezionare inviare attestazioni mediante una regola personalizzata e quindi fare clic su Avanti.
5. Nella pagina Configura regola sotto Nome regola attestazione, digitare il nome visualizzato per questa regola. In regola personalizzata, digitare o incollare la seguente sintassi del linguaggio di regola attestazione: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Fare clic su Fine. Verificare che la nuova regola venga visualizzato immediatamente sotto il Consenti accesso a regole di tutti gli utenti nell'elenco delle regole di autorizzazione di rilascio.
7. Per salvare la regola, nella finestra di dialogo Modifica regole attestazione, fare clic su OK.

>[!NOTE]
>Dovrai sostituire il valore di sopra di "espressione regolare indirizzo ip pubblico" con un'espressione di IP valida. vedere la compilazione dell'espressione di intervallo di indirizzi IP per altre informazioni.

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>Scenario 3: Bloccare completamente l'accesso esterno a Office 365, ad eccezione di applicazioni basate su browser

Il set di regole si basa sulla regola di autorizzazione rilascio predefinita denominata Consenti accesso a tutti gli utenti. Usare la procedura seguente per aggiungere una regola di autorizzazione di rilascio per la piattaforma delle identità Microsoft Office 365 trust della relying party usando la procedura guidata regola di attestazione:

>[!NOTE]
>Questo scenario non è supportato con un proxy di terze parti a causa delle limitazioni nelle intestazioni dei criteri di accesso client con richieste passive (basata sul Web).

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Per creare una regola per bloccare tutto l'accesso esterno a Office 365, ad eccezione di applicazioni basate su browser



1. Fare clic su Start, scegliere Programmi, strumenti di amministrazione e quindi fare clic su AD FS 2.0 Management.
2. Nell'albero della console, in AD FS 2.0\Trust Relationships, fare clic su trust Relying Party e quindi fare clic su Edit Claim Rules fare doppio clic su trust della piattaforma delle identità di Microsoft Office 365. 
3. Nella finestra di dialogo Modifica regole attestazione, selezionare la scheda regole di autorizzazione rilascio e quindi fare clic su Aggiungi regola per avviare la creazione guidata regola di attestazione.
4. Nella pagina Seleziona modello di regola, in modello di regola attestazione, selezionare inviare attestazioni mediante una regola personalizzata e quindi fare clic su Avanti.
5. Nella pagina Configura regola sotto Nome regola attestazione, digitare il nome visualizzato per questa regola. In regola personalizzata, digitare o incollare la seguente sintassi del linguaggio di regola attestazione: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Fare clic su Fine. Verificare che la nuova regola venga visualizzato immediatamente sotto il Consenti accesso a regole di tutti gli utenti nell'elenco delle regole di autorizzazione di rilascio.
7. Per salvare la regola, nella finestra di dialogo Modifica regole attestazione, fare clic su OK.

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Scenario 4: Bloccare completamente l'accesso esterno a Office 365 per i gruppi di Active Directory designati

Nell'esempio seguente Abilita l'accesso da client interni basate sull'indirizzo IP. Blocca l'accesso dai client che si trovano all'esterno della rete azienda che hanno un indirizzo IP del client esterni, fatta eccezione per gli utenti in una regola gruppo di Active Directory specifica set si basa sulla regola di autorizzazione rilascio predefinita denominata Consenti accesso a Tutti gli utenti. Usare la procedura seguente per aggiungere una regola di autorizzazione di rilascio per la piattaforma delle identità Microsoft Office 365 trust della relying party usando la procedura guidata regola di attestazione:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Per creare una regola per bloccare tutto l'accesso esterno a Office 365 per i gruppi di Active Directory designati



1. Fare clic su Start, scegliere Programmi, strumenti di amministrazione e quindi fare clic su AD FS 2.0 Management.
2. Nell'albero della console, in AD FS 2.0\Trust Relationships, fare clic su trust Relying Party e quindi fare clic su Edit Claim Rules fare doppio clic su trust della piattaforma delle identità di Microsoft Office 365. 
3. Nella finestra di dialogo Modifica regole attestazione, selezionare la scheda regole di autorizzazione rilascio e quindi fare clic su Aggiungi regola per avviare la creazione guidata regola di attestazione.
4. Nella pagina Seleziona modello di regola, in modello di regola attestazione, selezionare inviare attestazioni mediante una regola personalizzata e quindi fare clic su Avanti.
5. Nella pagina Configura regola sotto Nome regola attestazione, digitare il nome visualizzato per questa regola. In regola personalizzata, digitare o incollare la seguente sintassi del linguaggio di regola attestazione: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Fare clic su Fine. Verificare che la nuova regola venga visualizzato immediatamente sotto il Consenti accesso a regole di tutti gli utenti nell'elenco delle regole di autorizzazione di rilascio.
7. Per salvare la regola, nella finestra di dialogo Modifica regole attestazione, fare clic su OK.


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>Descrizioni della sintassi del linguaggio di regola attestazione usata negli scenari precedenti

|Descrizione|Sintassi del linguaggio di regola attestazione|
|-----|-----| 
|Regola di predefinita di AD FS per consentire l'accesso a tutti gli utenti. Questa regola deve già esistere in Office 365 piattaforma delle identità Microsoft trust della relying party elenco di regole di autorizzazione rilascio.|=> issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");| 
|Aggiunta a una regola personalizzata di questa clausola specifica che la richiesta proviene da proxy server federativo (ad esempio, con l'intestazione x-ms-proxy)
È consigliabile che tutte le regole di includano ciò.|exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"])| 
|Usato per stabilire che la richiesta proviene da un client con un indirizzo IP compreso nell'intervallo accettabile definito.|NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value=~"customer-provided public ip address regex"])| 
|Questa clausola viene utilizzata per specificare che, se l'applicazione a cui si accede non Microsoft.Exchange.ActiveSync la richiesta deve essere negata.|NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value=="Microsoft.Exchange.ActiveSync"])| 
|Questa regola consente di determinare se la chiamata è stato impostato tramite un Web browser e non verrà negata.|NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])| 
|Questa regola indica che gli utenti soli in un determinato gruppo di Active Directory (basato su valore SID) devono essere rifiutati. Aggiunta di questa istruzione non significa che un gruppo di utenti sarà consentito, indipendentemente dalla posizione.|exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "{Group SID value of allowed AD group}"])| 
|Si tratta di una clausola necessaria per emettere un'istruzione deny quando vengono soddisfatte tutte le condizioni precedenti.|=> issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");|

### <a name="building-the-ip-address-range-expression"></a>La compilazione dell'espressione di intervallo di indirizzi IP

L'attestazione di x-ms-inoltrati--indirizzo ip del client viene popolato da un'intestazione HTTP che è attualmente impostata solo per Exchange Online, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione per AD FS. Il valore dell'attestazione può essere uno dei seguenti:

>[!Note] 
>Exchange Online supporta attualmente solo indirizzi IPV4 e non su IPV6.

Un singolo indirizzo IP: L'indirizzo IP del client che è direttamente connesso a Exchange Online

>[!Note] 
>L'indirizzo IP di un client nella rete aziendale verrà visualizzato come indirizzo IP interfaccia esterna del proxy in uscita dall'organizzazione o un gateway.

I client connessi alla rete aziendale tramite una VPN o da Microsoft DirectAccess (DA) potrebbero apparire come client aziendale interno o come i client esterni a seconda della configurazione della VPN o DA.

Uno o più indirizzi IP: Quando Exchange Online non è possibile determinare l'indirizzo IP del client, imposterà il valore in base al valore dell'intestazione x-forwarded-for, un'intestazione non standard che può essere inclusi in richieste basate su HTTP ed è supportata da molti client, i bilanciamenti del carico, e dai proxy sul mercato.

>[!Note]
>Più indirizzi IP, che indica l'indirizzo di ogni proxy che la richiesta, passati e indirizzo IP del client saranno separati da una virgola.

Gli indirizzi IP correlati all'infrastruttura Exchange Online non verranno visualizzati nell'elenco.


#### <a name="regular-expressions"></a>Espressioni regolari

Quando si dispone per la corrispondenza di un intervallo di indirizzi IP, diventa necessario costruire un'espressione regolare per eseguire il confronto. Nella serie successiva di questa procedura, concetto verranno forniti esempi per creare tale espressione in modo da corrispondere gli intervalli di indirizzi seguente (si noti che è necessario modificare questi esempi in modo da corrispondere l'intervallo di IP pubblici):


- 192.168.1.1 – 192.168.1.25
- 10.0.0.1 – 10.0.0.14

In primo luogo, il modello di base che corrisponderà a un singolo indirizzo IP è il seguente: \b###\.###\.###\.\b # # #

Questa estensione, è possibile associare come indicato di seguito due diversi indirizzi IP con un'espressione OR: \b###\.###\.###\.# # # \b|\b###\. ### \. ### \.\b # # #

Pertanto, un esempio in modo che corrisponda solo due indirizzi (ad esempio 192.168.1.1 o 10.0.0.1) sarebbe: \b192\.168\.1\.1\b | \b10\.0\.0\.1\b

Questo offre la tecnica mediante il quale è possibile immettere qualsiasi numero di indirizzi. In cui è necessario un intervallo di indirizzi consentiti, ad esempio 192.168.1.1: 192.168.1.25, la corrispondenza è necessario eseguire carattere per carattere: \b192\.168\.1\.([1-9] | [0-9] 1 | 2 [0-5]) \b

>[!Note] 
>L'indirizzo IP viene considerato come stringa e non un numero.


La regola è suddiviso come indicato di seguito: \b192\.168\.1\.

Ciò corrisponde a inizio qualsiasi valore con 192.168.1.

Gli intervalli dopo il separatore decimale finale necessari per la parte dell'indirizzo corrisponde a quanto segue:


- ([1-9] abbina gli indirizzi che terminano con 1 a 9
- | 1 [0-9] corrisponde agli indirizzi che terminano con 10-19
- |2[0-5]) abbina gli indirizzi che terminano con 20-25

>[!Note]
>Le parentesi devono essere posizionate in modo corretto, in modo da non avviare altre parti di indirizzi IP corrispondenti.

Con il blocco di 192 corrispondente, è possibile scrivere un'espressione simile per il blocco di 10: \b10\.0\.0\.([1-9] | [0-4] di 1) \b

E inserendoli insieme, l'espressione seguente deve corrispondere tutti gli indirizzi per "192.168.1.1~25" e "10.0.0.1~14": \b192\.168\.1\.([1-9] | [0-9] 1 | 2 [0-5]) \b|\b10\.0\.0\. ([1-9] | [0-4] di 1) \b

#### <a name="testing-the-expression"></a>L'espressione di test

Le espressioni di espressione regolare possono diventare piuttosto complicate, pertanto è consigliabile usare uno strumento di verifica di espressione regolare. Se si esegue una ricerca in internet per "builder espressione regex online", troverai diverse buona utilità online che ti permetterà di provare le espressioni in base a dati di esempio.

Quando l'espressione di test, è importante comprendere che cosa si aspettano di avere in modo che corrispondano. Il sistema di Exchange online può inviare numero di indirizzi IP, separati da virgole. Le espressioni fornite in precedenza funzionerà per questo oggetto. Tuttavia, è importante considerare questo durante il test di espressioni di espressione regolare. Uno potrebbe ad esempio, usare l'esempio seguente di input per verificare gli esempi precedenti: 

192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20 

10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1 











## <a name="validating-the-deployment"></a>La convalida della distribuzione

### <a name="security-audit-logs"></a>Log di controllo di sicurezza

Per verificare che il nuovo contesto di richiesta delle attestazioni vengono inviate e sono disponibili per AD FS che elabora la pipeline di attestazioni, abilitare il controllo di accesso al server AD FS. Inviare quindi alcune richieste di autenticazione e controllo per i valori di attestazione nelle voci di log di controllo di sicurezza standard. 

Per abilitare la registrazione di controllo del registro degli eventi per la sicurezza in un server AD FS, seguire i passaggi descritti in Configura il controllo per AD FS 2.0.

### <a name="event-logging"></a>Registrazione degli eventi

Per impostazione predefinita, le richieste non riuscite vengono registrate nel registro eventi dell'applicazione che si trova in registri applicazioni e servizi \ AD FS 2.0 \ vedere altre informazioni sulla registrazione di eventi per AD FS, Admin.For [configurare AD la registrazione degli eventi di ADFS 2.0](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>Configurazione di ADFS Verbose log di traccia

Gli eventi di traccia di AD FS vengono registrati nel Registro di AD FS 2.0 debug. Per abilitare la traccia, vedere [configurare la traccia di debug per AD FS 2.0](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

Dopo aver abilitato la traccia, usare la sintassi della riga di comando seguente per abilitare il livello di registrazione dettagliata: wevtutil.exe sl "ADFS 2.0 traccia/Debug" /l:5  

## <a name="related"></a>Risorse correlate
Per altre informazioni sui nuovi tipi di attestazione, vedere [tipi di attestazioni di AD FS](AD-FS-Claims-Types.md).
 
