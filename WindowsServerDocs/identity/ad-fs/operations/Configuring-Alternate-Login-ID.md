---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: Configurazione di un ID di accesso alternativo
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7e7a881a2e6bae499ed7d4713bd70a804c3412e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816964"
---
# <a name="configuring-alternate-login-id"></a>Configurazione di un ID di accesso alternativo


## <a name="what-is-alternate-login-id"></a>Che cos'è l'ID di accesso alternativo?
Nella maggior parte degli scenari, gli utenti usano il proprio UPN (nomi dell'entità utente) per accedere ai propri account. Tuttavia, in alcuni ambienti a causa dei criteri aziendali o delle dipendenze delle applicazioni line-of-business locali, gli utenti potrebbero usare un altro tipo di accesso. 

>[!NOTE]
>Le procedure consigliate di Microsoft sono la corrispondenza tra UPN e indirizzo SMTP primario. In questo articolo viene illustrata la piccola percentuale di clienti che non è in grado di correggere gli UPN per la corrispondenza.

Ad esempio, possono usare il proprio ID di posta elettronica per l'accesso e possono essere diversi dall'UPN. Si tratta in genere di un'occorrenza comune negli scenari in cui il nome UPN non è instradabile. Si consideri un utente Jane Doe con UPN jdoe@contoso.local e indirizzo di posta elettronica jdoe@contoso.com. Jane potrebbe non essere a conoscenza dell'UPN perché ha sempre usato il proprio ID di posta elettronica per l'accesso. L'utilizzo di qualsiasi altro metodo di accesso anziché UPN costituisce un ID alternativo. Per ulteriori informazioni sulla creazione dell'UPN, vedere [Azure ad popolamento di userPrincipalName](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-userprincipalname).

Active Directory Federation Services (AD FS) consente alle applicazioni federate che usano AD FS di accedere con l'ID alternativo. Ciò consente agli amministratori di specificare un'alternativa al nome UPN predefinito da usare per l'accesso. AD FS supporta già l'utilizzo di qualsiasi forma di identificatore utente accettato da Active Directory Domain Services (AD DS). Quando è configurato per l'ID alternativo, AD FS consente agli utenti di accedere usando il valore dell'ID alternativo configurato, ad indicare email-ID. L'uso dell'ID alternativo consente di adottare provider SaaS, ad esempio Office 365 senza modificare la UPN locale. Consente inoltre di supportare le applicazioni di servizio line-of-business con le identità con provisioning degli utenti.

## <a name="alternate-id-in-azure-ad"></a>ID alternativo in Azure AD
Un'organizzazione potrebbe dover utilizzare un ID alternativo negli scenari seguenti:
1. Il nome di dominio locale non è instradabile, ad esempio Contoso. local e, di conseguenza, il nome dell'entità utente predefinito è non instradabile (jdoe@contoso.local). Non è possibile modificare il nome UPN esistente a causa di dipendenze dell'applicazione locale o criteri aziendali. Azure AD e Office 365 richiedono che tutti i suffissi di dominio associati alla directory Azure AD siano completamente instradabili tramite Internet. 
2. L'UPN locale non è uguale all'indirizzo di posta elettronica dell'utente e per accedere a Office 365, gli utenti usano l'indirizzo di posta elettronica e l'UPN non possono essere usati a causa di vincoli organizzativi.
   Negli scenari indicati in precedenza, l'ID alternativo con AD FS consente agli utenti di accedere a Azure AD senza modificare la UPN locale. 

## <a name="end-user-experience-with-alternate-login-id"></a>Esperienza dell'utente finale con ID di accesso alternativo
L'esperienza dell'utente finale varia a seconda del metodo di autenticazione utilizzato con l'ID di accesso alternativo.  Attualmente esistono tre modi diversi in cui è possibile ottenere l'utilizzo di un ID di accesso alternativo.  ovvero:

- **Autenticazione normale (legacy)** : usa il protocollo di autenticazione di base.
- **Autenticazione moderna** : consente l'accesso basato su Active Directory Authentication Library (adal) alle applicazioni. Questo consente funzionalità di accesso come Multi-Factor Authentication (multi-factor authentication), provider di identità di terze parti basati su SAML con applicazioni client Office, smart card e autenticazione basata su certificati.
- **Autenticazione moderna ibrida** : offre tutti i vantaggi dell'autenticazione moderna e fornisce agli utenti la possibilità di accedere alle applicazioni locali usando i token di autorizzazione ottenuti dal cloud.

>[!NOTE]
> Per la migliore esperienza possibile, Microsoft consiglia vivamente l'autenticazione ibrida moderna.



## <a name="configure-alternate-logon-id"></a>Configurare l'ID di accesso alternativo
Utilizzando Azure AD Connect è consigliabile utilizzare Azure AD Connetti per configurare un ID di accesso alternativo per l'ambiente.

- Per una nuova configurazione di Azure AD Connect, vedere Connetti a Azure AD per istruzioni dettagliate su come configurare l'ID alternativo e AD FS farm.
- Per le installazioni di Azure AD Connect esistenti, vedere Modifica del metodo di accesso utente per istruzioni sulla modifica del metodo di accesso per AD FS

Azure AD Connect quando vengono fornite informazioni dettagliate sull'ambiente AD FS, viene verificata automaticamente la presenza della parte destra del AD FS e viene configurata AD FS per l'ID alternativo, incluse tutte le regole attestazioni corrette appropriate per Azure AD trust federativa. Non sono necessari passaggi aggiuntivi per configurare l'ID alternativo.

>[!NOTE]
> Microsoft consiglia di utilizzare Azure AD Connect per configurare l'ID di accesso alternativo.

### <a name="manually-configure-alternate-id"></a>Configurare manualmente l'ID alternativo
Per configurare un ID di accesso alternativo, è necessario eseguire le attività seguenti: configurare i trust del provider di attestazioni AD FS per abilitare l'ID di accesso alternativo

1.  Se si dispone di un server 2012R2, verificare che KB2919355 sia installato in tutti i server AD FS. È possibile ottenerlo tramite Windows Update Services o scaricarlo direttamente. 

2.  Aggiornare la configurazione del AD FS eseguendo il cmdlet di PowerShell seguente in uno qualsiasi dei server federativi della farm (se si dispone di una farm WID, è necessario eseguire questo comando nel server di AD FS primario della farm):

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>
```

**AlternateLoginID** è il nome LDAP dell'attributo che si desidera utilizzare per l'accesso.

**LookupForests** è l'elenco di DNS della foresta a cui appartengono gli utenti.

Per abilitare la funzionalità ID di accesso alternativo, è necessario configurare i parametri-AlternateLoginID e-LookupForests con un valore non null e valido.

Nell'esempio seguente viene abilitata la funzionalità di ID di accesso alternativo in modo che gli utenti con account nelle foreste contoso.com e fabrikam.com possano accedere alle applicazioni abilitate per AD FS con l'attributo "mail".

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
```

3. Per disabilitare questa funzionalità, impostare il valore di entrambi i parametri su null.

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
```

## <a name="hybrid-modern-authentication-with-alternate-id"></a>Autenticazione moderna ibrida con ID alternativo

>[!IMPORTANT]
>Il codice seguente è stato testato solo per AD FS e non per i provider di identità di terze parti.

### <a name="exchange-and-skype-for-business"></a>Exchange e Skype for business
Se si usa un ID di accesso alternativo con Exchange e Skype for business, l'esperienza utente varia a seconda che si usi o meno HMA.

>[!NOTE]
>Per ottimizzare l'esperienza dell'utente finale, Microsoft consiglia di usare l'autenticazione ibrida moderna.

per altre informazioni, vedere [Panoramica dell'autenticazione moderna ibrida](https://support.office.com/article/Hybrid-Modern-Authentication-overview-and-prerequisites-for-using-it-with-on-premises-Skype-for-Business-and-Exchange-servers-ef753b32-7251-4c9e-b442-1a5aec14e58d)

### <a name="pre-requisites-for-exchange-and-skype-for-business"></a>Prerequisiti per Exchange e Skype for business
Di seguito sono riportati i prerequisiti per ottenere l'accesso SSO con ID alternativo.

- PER Exchange Online dovrebbe essere attivata l'autenticazione moderna.
- Per Skype for business (SFB) online dovrebbe essere attivata l'autenticazione moderna.
- Per Exchange locale dovrebbe essere attivata l'autenticazione moderna.  Exchange 2013 CU19 o Exchange 2016 CU18 è necessario su tutti i server Exchange. Nessun Exchange 2010 nell'ambiente.
- Skype for business locale deve avere l'autenticazione moderna attivata.
- È necessario usare i client di Exchange e Skype con l'autenticazione moderna abilitata. Tutti i server devono eseguire SFB server 2015 CU5.
- Client Skype for business che supportano l'autenticazione moderna
   - iOS, Android, Windows Phone
   - SFB 2016 (MA è attiva per impostazione predefinita, ma assicurarsi che non sia stata disabilitata).
   - SFB 2013 (MA è disattivato per impostazione predefinita, quindi verificare che sia stato attivato).
   - SFB Mac desktop
- Client di Exchange che supportano l'autenticazione moderna e supportano REGKEYS AltI
    - Solo Office Pro Plus 2016





#### <a name="supported-office-version"></a>Versione di Office supportata

La configurazione della directory per SSO con ID alternativo con ID alternativo può causare richieste aggiuntive per l'autenticazione se queste configurazioni aggiuntive non sono state completate. Per un possibile effetto sull'esperienza utente con ID alternativo, vedere l'articolo.

Con la configurazione aggiuntiva seguente, l'esperienza utente è stata migliorata in modo significativo ed è possibile ottenere richieste quasi nulle per l'autenticazione per gli utenti di ID alternativo nell'organizzazione.

##### <a name="step-1-update-to-required-office-version"></a>Passaggio 1. Aggiornamento alla versione di Office richiesta
Office versione 1712 (build No 8827,2148) e versioni successive hanno aggiornato la logica di autenticazione per gestire lo scenario di ID alternativo. Per sfruttare la nuova logica, i computer client devono essere aggiornati a Office versione 1712 (build No 8827,2148) e versioni successive.

##### <a name="step-2-update-to-required-windows-version"></a>Passaggio 2. Aggiornamento alla versione richiesta di Windows
Windows versione 1709 e successive hanno aggiornato la logica di autenticazione per gestire lo scenario di ID alternativo. Per sfruttare la nuova logica, i computer client devono essere aggiornati a Windows versione 1709 e successive.

##### <a name="step-3-configure-registry-for-impacted-users-using-group-policy"></a>Passaggio 3. Configurare il registro di sistema per utenti interessati tramite criteri di gruppo
Le applicazioni di Office si basano sulle informazioni inviate dall'amministratore di directory per identificare l'ambiente di ID alternativo. Le chiavi del registro di sistema seguenti devono essere configurate per consentire alle applicazioni di Office di autenticare l'utente con ID alternativo senza visualizzare richieste aggiuntive

|Chiave da aggiungere|Chiave nome dati, tipo e valore|Windows 7/8|Windows 10|Descrizione|
|-----|-----|-----|-----|-----|
|HKEY_CURRENT_USER \Software\Microsoft\AuthN|DomainHint</br>REG_SZ</br>contoso.com|Obbligatoria|Obbligatoria|Il valore di questo chiave è un nome di dominio personalizzato verificato nel tenant dell'organizzazione. Ad esempio, Contoso Corp può fornire un valore Contoso.com in questo chiave se Contoso.com è uno dei nomi di dominio personalizzati verificati nel tenant Contoso.onmicrosoft.com.|
HKEY_CURRENT_USER \Software\Microsoft\Office\16.0\Common\Identity|EnableAlternateIdSupport</br>REG_DWORD</br>1|Obbligatorio per Outlook 2016 ProPlus|Obbligatorio per Outlook 2016 ProPlus|Il valore di questo chiave può essere 1/0 per indicare all'applicazione Outlook se deve coinvolgere la logica di autenticazione con ID alternativo migliorata.|
HKEY_CURRENT_USER \Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\contoso.com\sts|&#42;</br>REG_DWORD</br>1|Obbligatoria|Obbligatoria|Questo chiave può essere usato per impostare il servizio token di accesso come zona attendibile nelle impostazioni Internet. La distribuzione standard di ADFS consiglia di aggiungere lo spazio dei nomi ADFS all'area Intranet locale per Internet Explorer|

## <a name="new-authentication-flow-after-additional-configuration"></a>Nuovo flusso di autenticazione dopo una configurazione aggiuntiva

![flusso di autenticazione](media/Configure-Alternate-Login-ID/alt1a.png)

1. a: l'utente viene sottoposta a provisioning in Azure AD usando l'ID alternativo
   </br>b: l'amministratore della directory inserisce le impostazioni chiave necessarie per i computer client interessati
2. L'utente esegue l'autenticazione nel computer locale e apre un'applicazione di Office
3. L'applicazione di Office accetta le credenziali della sessione locale
4. L'applicazione di Office esegue l'autenticazione per Azure AD usando l'hint di dominio push dall'amministratore e le credenziali locali
5. Azure AD autentica l'utente indirizzando l'area di autenticazione corretta e rilascia un token

## <a name="applications-and-user-experience-after-the-additional-configuration"></a>Applicazioni ed esperienza utente dopo la configurazione aggiuntiva

### <a name="non-exchange-and-skype-for-business-clients"></a>Client non Exchange e Skype for business

|Client|Istruzione support|Note|
| ----- | -----|-----|
|Microsoft Teams|Supportato|<li>Microsoft teams supporta AD FS (SAML-P, WS-Fed, WS-Trust e OAuth) e l'autenticazione moderna.</li><li> I principali team Microsoft, ad esempio canali, chat e funzionalità di file, funzionano con l'ID di accesso alternativo.</li><li>le app 1st e di terze parti devono essere esaminate separatamente dal cliente. Ciò è dovuto al fatto che ogni applicazione dispone di protocolli di autenticazione del supporto.</li>|     
|OneDrive for Business|Supportato: chiave del registro di sistema lato client consigliata |Con l'ID alternativo configurato è possibile vedere che l'UPN locale è già popolato nel campo di verifica. Questa operazione deve essere modificata nell'identità alternativa utilizzata. Si consiglia di usare la chiave del registro di sistema lato client indicata in questo articolo: Office 2013 e Lync 2013 richiedono periodicamente le credenziali per SharePoint Online, OneDrive e Lync Online.|
|Client per dispositivi mobili OneDrive for business|Supportato|| 
|Pagina di attivazione di Office 365 Pro Plus|Supportato: chiave del registro di sistema lato client consigliata|Con l'ID alternativo configurato è possibile vedere che l'UPN locale è già popolato nel campo di verifica. Questa operazione deve essere modificata nell'identità alternativa utilizzata. Si consiglia di usare la chiave del registro di sistema lato client indicata in questo articolo: Office 2013 e Lync 2013 richiedono periodicamente le credenziali per SharePoint Online, OneDrive e Lync Online.|

### <a name="exchange-and-skype-for-business-clients"></a>Client Exchange e Skype for business

|Client|Istruzione di supporto-con HMA|Istruzione di supporto-senza HMA|
| ----- |----- | ----- |
|Outlook|Supportato, nessun prompt aggiuntivo|Supportato</br></br>Con **l'autenticazione moderna** per Exchange Online: supportato</br></br>Con **l'autenticazione normale** per Exchange Online: supportata con le avvertenze seguenti:</br><li>È necessario trovarsi in un computer aggiunto a un dominio e connettersi alla rete aziendale </li><li>È possibile utilizzare l'ID alternativo solo negli ambienti che non consentono l'accesso esterno per gli utenti delle cassette postali. Questo significa che gli utenti possono eseguire l'autenticazione alla propria cassetta postale in modo supportato quando sono connessi e aggiunti alla rete aziendale, in una VPN o connessi tramite computer ad accesso diretto, ma quando si configura il profilo di Outlook si ottengono due richieste aggiuntive.| 
|Cartelle pubbliche ibride|Supportato, nessuna richiesta aggiuntiva.|Con **l'autenticazione moderna** per Exchange Online: supportato</br></br>Con **autenticazione normale** per Exchange Online: non supportato</br></br><li>Le cartelle pubbliche ibride non possono espandersi se vengono usati ID alternativi e pertanto non devono essere usati oggi con metodi di autenticazione regolari.|
|Delega cross-premise|Vedere [configurare Exchange per supportare le autorizzazioni delle cassette postali delegate in una distribuzione ibrida](https://technet.microsoft.com/library/mt784505.aspx)|Vedere [configurare Exchange per supportare le autorizzazioni delle cassette postali delegate in una distribuzione ibrida](https://technet.microsoft.com/library/mt784505.aspx)|
|Archiviare l'accesso alle cassette postali (cassetta postale locale-archivio nel cloud)|Supportato, nessun prompt aggiuntivo|Supportato: gli utenti ricevono un prompt aggiuntivo per le credenziali quando accedono all'archivio, devono fornire il proprio ID alternativo quando richiesto.| 
|Outlook Web Access|Supportato|Supportato|
|App Outlook per dispositivi mobili per Android, IOS e Windows Phone|Supportato|Supportato|
|Skype for business/Lync|Supportato, senza richieste aggiuntive|Supportato (ad eccezione di quanto indicato), ma è possibile che si verifichi una confusione utente.</br></br>Nei client mobili, l'ID alternativo è supportato solo se l'indirizzo SIP = indirizzo di posta elettronica = ID alternativo.</br></br> È possibile che gli utenti debbano eseguire due volte l'accesso a Skype for business desktop client usando il nome UPN locale e quindi usando l'ID alternativo. (Si noti che il "indirizzo di accesso" è in realtà l'indirizzo SIP che potrebbe non corrispondere al "nome utente", anche se spesso è). Quando viene richiesto per la prima volta un nome utente, l'utente deve immettere l'UPN, anche se non viene prepopolato erroneamente con l'ID alternativo o l'indirizzo SIP. Dopo che l'utente ha fatto clic su Accedi con l'UPN, la richiesta del nome utente viene nuovamente visualizzata e questa volta viene prepopolato con l'UPN. Questa volta l'utente deve sostituire con l'ID alternativo e fare clic su Accedi per completare il processo di accesso. Nei client mobili, gli utenti devono immettere l'ID utente locale nella pagina avanzate, usando il formato di tipo SAM (dominio\nomeutente) e non il formato UPN.</br></br>Dopo aver eseguito l'accesso, se Skype for business o Lync indica che "Exchange richiede le credenziali", è necessario fornire le credenziali valide per la posizione della cassetta postale. Se la cassetta postale si trova nel cloud, è necessario fornire l'ID alternativo. Se la cassetta postale è locale, è necessario fornire l'UPN locale.| 

## <a name="additional-details--considerations"></a>Dettagli aggiuntivi & considerazioni

-   La funzionalità ID di accesso alternativo è disponibile per gli ambienti federati con AD FS distribuito.  Non è supportata negli scenari seguenti:
    -   Domini non instradabili (ad esempio contoso. local) che non possono essere verificati dal Azure AD.
    -   Ambienti gestiti in cui non è AD FS distribuito.


-   Se abilitata, la funzionalità ID di accesso alternativo è disponibile solo per l'autenticazione di nome utente/password in tutti i protocolli di autenticazione nome utente/password supportati da AD FS (SAML-P, WS-Fed, WS-Trust e OAuth).


-   Quando viene eseguita l'autenticazione integrata di Windows (WIA), ad esempio quando gli utenti tentano di accedere a un'applicazione aziendale in un computer aggiunto a un dominio dalla rete Intranet e AD FS amministratore ha configurato i criteri di autenticazione per l'utilizzo di WIA per Intranet), UPN utilizzato per l'autenticazione. Se sono state configurate regole attestazioni per la funzionalità di ID di accesso alternativo, è necessario assicurarsi che tali regole siano ancora valide nel caso di WIA.

-   Se abilitata, la funzionalità ID di accesso alternativo richiede che almeno un server di catalogo globale sia raggiungibile dal server AD FS per ogni foresta di account utente supportata da AD FS. L'impossibilità di raggiungere un server di catalogo globale nella foresta di account utente comporta AD FS il fallback all'utilizzo dell'UPN. Per impostazione predefinita, tutti i controller di dominio sono server di catalogo globale.

-   Se abilitata, se il server AD FS trova più di un oggetto utente con lo stesso valore di ID di accesso alternativo specificato in tutte le foreste di account utente configurato, l'accesso non riesce.

-   Quando è abilitata la funzionalità ID di accesso alternativo, AD FS prova a eseguire prima l'autenticazione dell'utente finale con ID di accesso alternativo e quindi esegue il fallback per usare UPN se non riesce a trovare un account che possa essere identificato dall'ID di accesso alternativo. È necessario assicurarsi che non ci siano conflitti tra l'ID di accesso alternativo e l'UPN se si vuole ancora supportare l'accesso UPN. Ad esempio, l'impostazione di un attributo mail con l'UPN dell'altro impedisce all'altro utente di accedere con il proprio UPN.

-   Se una delle foreste configurate dall'amministratore è inattiva, AD FS continua a cercare l'account utente con ID di accesso alternativo in altre foreste configurate. Se AD FS server trova gli oggetti utente univoci nelle foreste in cui è stata eseguita la ricerca, un utente esegue correttamente l'accesso.

-   Potrebbe inoltre essere necessario personalizzare la pagina di accesso AD FS per fornire agli utenti finali un suggerimento sull'ID di accesso alternativo. È possibile eseguire questa operazione aggiungendo la descrizione della pagina di accesso personalizzata (per altre informazioni, vedere [personalizzazione delle pagine di accesso ad FS](https://technet.microsoft.com/library/dn280950.aspx) o personalizzazione della stringa di accesso con account aziendale sopra il campo nome utente (per altre informazioni, vedere [personalizzazione avanzata delle pagine di accesso ad FS](https://technet.microsoft.com/library/dn636121.aspx).

-   Il nuovo tipo di attestazione che contiene il valore di ID di accesso alternativo è **http: schemas. Microsoft. com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>Contatori di eventi e prestazioni
Sono stati aggiunti i contatori delle prestazioni seguenti per misurare le prestazioni dei server AD FS quando è abilitato l'ID di accesso alternativo:

-   Autenticazioni ID di accesso alternativo: numero di autenticazioni eseguite utilizzando un ID di accesso alternativo

-   Autenticazione ID di accesso alternativo/sec: numero di autenticazioni eseguite utilizzando un ID di accesso alternativo al secondo

-   Latenza di ricerca media per ID di accesso alternativo: latenza di ricerca media tra le foreste configurate da un amministratore per l'ID di accesso alternativo

Di seguito sono riportati diversi casi di errore e il relativo effetto sull'esperienza di accesso dell'utente con gli eventi registrati da AD FS:



|                       **Casi di errore**                        | **Effetti sull'esperienza di accesso** |                                                              **Evento**                                                              |
|--------------------------------------------------------------|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| Impossibile ottenere un valore per SAMAccountName per l'oggetto utente |          Errore di accesso           |                  ID evento 364 con messaggio eccezione MSIS8012: Impossibile trovare samAccountName per l'utente:'{0}'.                   |
|        L'attributo CanonicalName non è accessibile         |          Errore di accesso           |               ID evento 364 con messaggio eccezione MSIS8013: canonicalName:'{0}' dell'utente:'{1}' è in formato non valido.                |
|        Sono presenti più oggetti utente in una foresta        |          Errore di accesso           | ID evento 364 con messaggio eccezione MSIS8015: sono stati trovati più account utente con identità'{0}' nella foresta '{1}' con identità: {2} |
|   Sono stati trovati più oggetti utente in più foreste    |          Errore di accesso           |           ID evento 364 con messaggio eccezione MSIS8014: sono stati trovati più account utente con identità'{0}' nelle foreste: {1}            |

## <a name="see-also"></a>Vedi anche
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)


