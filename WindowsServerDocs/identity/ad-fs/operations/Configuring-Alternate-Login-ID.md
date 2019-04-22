---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: Configurazione di un ID di accesso alternativo
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 615faf4153949aa4ad989f017068d1809fca26b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820872"
---
# <a name="configuring-alternate-login-id"></a>Configurazione di un ID di accesso alternativo

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

## <a name="what-is-alternate-login-id"></a>Che cos'è l'ID di accesso alternativo?
Nella maggior parte degli scenari, gli utenti usano il proprio UPN (nomi dell'entità utente) all'account di accesso agli account. Tuttavia, in alcuni ambienti a causa dei criteri aziendali o dipendenze delle applicazioni line-of-business locali, gli utenti stia usando un'altra forma di accesso. 

>[!NOTE]
>Sono consigliate in modo che corrisponda all'indirizzo SMTP primario UPN consigliata da Microsoft. Questo articolo fornisce informazioni piccola percentuale di clienti che non è possibile correggere UPN in modo che corrispondano.

Ad esempio, è possibile che utilizzino il relativo id di posta elettronica per l'accesso e che può essere diverso dal relativo UPN. Si tratta in particolare un problema comune negli scenari in cui il nome dell'entità non è instradabile. Si consideri un utente Valeria dal monte con UPN jdoe@contoso.local e indirizzo di posta elettronica jdoe@contoso.com. Jane potrebbe non essere neanche a conoscenza dell'UPN come Anna è sempre stato usato l'id di posta elettronica per l'accesso. Uso di qualsiasi altro metodo di accesso invece di UPN costituisce un ID alternativo. Per altre informazioni sul modo in cui l'UPN è Crea, vedere [popolamento di UserPrincipalName di Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-userprincipalname).

ID di Active Directory Federation Services (ADFS) Abilita applicazioni federate tramite AD FS per l'accesso tramite alternativo. Ciò consente agli amministratori di specificare un'alternativa all'impostazione predefinita UPN da utilizzare per l'accesso. ADFS supporta già usando qualsiasi formato dell'ID utente che viene accettata da Active Directory Domain Services (AD DS). Quando è configurato per l'ID alternativo, AD FS consente agli utenti di accedere usando il valore ID alternativo configurato, ad esempio id di posta elettronica. Usando l'ID alternativo consente di adottare i provider SaaS, ad esempio Office 365 senza modificare i nomi UPN locali. Consente inoltre di supportare le applicazioni di servizio line-of-business con le identità con provisioning consumer.

## <a name="alternate-id-in-azure-ad"></a>Id alternativo in Azure AD
Un'organizzazione potrebbe essere necessario usare un ID alternativo negli scenari seguenti:
1.  È non instradabile, ad esempio, il nome di dominio locale. Contoso. Local e di conseguenza il nome dell'entità utente predefinito è non instradabile (jdoe@contoso.local). UPN esistente non può essere modificato a causa di dipendenze dell'applicazione locale o criteri aziendali. Azure AD e Office 365 richiede tutti i suffissi di dominio associati alla directory di Azure AD essere completamente instradabile internet. 
2.  L'UPN locale non è lo stesso indirizzo di posta elettronica dell'utente e di accesso a Office 365, gli utenti usano l'indirizzo di posta elettronica e UPN non può essere usato a causa dei limiti dell'organizzazione.
Negli scenari sopra indicati, ID alternativo con AD FS consente agli utenti di accedere ad Azure AD senza modificare i nomi UPN locali. 

## <a name="end-user-experience-with-alternate-login-id"></a>Esperienza dell'utente finale con ID di accesso alternativo
L'esperienza utente finale varia a seconda del metodo di autenticazione usato con id di accesso alternativo.  Attualmente esiste tre diversi modi in cui possono essere realizzati con id di accesso alternativo.  Sono:

- **Autenticazione regolare (Legacy)**-utilizza il protocollo di autenticazione di base.
- **L'autenticazione moderna** -offre accesso aggiuntivo basato su Active Directory Authentication Library (adal) alle applicazioni. In questo modo funzionalità di accesso, ad esempio multi-Factor Authentication (MFA), basato su SAML dei provider di identità di terze parti con le applicazioni client di Office, smart card e l'autenticazione basata su certificato.
- **L'autenticazione moderna ibrida** : Elenca tutti i vantaggi dell'autenticazione moderna e offre agli utenti la possibilità di accedere alle applicazioni locali tramite i token di autorizzazione ottenuti dal cloud.

>[!NOTE]
> Per la migliore esperienza possibile, si consiglia l'autenticazione moderna ibrida.



## <a name="configure-alternate-logon-id"></a>Configurare l'ID di accesso alternativo
Usando Azure AD Connect è consigliabile usare Azure AD connect per configurare l'ID di accesso alternativo per l'ambiente.

- Per la nuova configurazione di Azure AD Connect, vedere Connect ad Azure AD per istruzioni dettagliate su come configurare una farm di ADFS di Active Directory e ID alternativo.
- Per le installazioni esistenti di Azure AD Connect, vedere Modifica del metodo di accesso utente per istruzioni sulla modifica metodo di accesso ad AD FS

Quando Azure AD Connect viene fornito informazioni dettagliate sull'ambiente di AD FS, verifica la presenza dei KB a destra in ADFS e configurato automaticamente AD FS per l'ID alternativo, incluse tutte le regole di attestazione destro necessari per il trust federativo Azure AD. Non è presente alcun passaggio aggiuntivo richiesto esterno Creazione guidata per configurare un ID alternativo.

>[!NOTE]
> Microsoft consiglia di usare Azure AD Connect per configurare l'ID di accesso alternativo.

### <a name="manually-configure-alternate-id"></a>Configurare manualmente un ID alternativo
Per configurare l'ID di accesso alternativo, è necessario eseguire le attività seguenti: Configurare il provider di attestazioni di ADFS per abilitare l'ID di accesso alternativo

1.  Se si dispone di Server 2012 R2, assicurarsi di che aver KB2919355 installato in tutti i server AD FS. È possibile ottenerlo tramite Windows Update Services o scaricarlo direttamente. 

2.  Aggiornare la configurazione di ADFS eseguendo il cmdlet di PowerShell seguente in uno qualsiasi dei server federativi della farm (se si dispone di una farm database interno di Windows, è necessario eseguire questo comando nel server ADFS primario nella farm):

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>
```

**AlternateLoginID** è il nome LDAP dell'attributo che si desidera usare per l'accesso.

**LookupForests** è riportato l'elenco di insiemi di strutture DNS che gli utenti appartengono a.

Per abilitare funzionalità di ID di accesso alternativo, è necessario configurare i parametri - AlternateLoginID sia - LookupForests con un valore diverso da null, valido.

Nell'esempio seguente, si abilita la funzionalità di ID di accesso alternativo in modo che gli utenti con account negli insiemi di strutture contoso.com e fabrikam.com possono accedere alle applicazioni di AD FS abilitata con il relativo attributo "mail".

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
```

3.  Per disabilitare questa funzionalità, impostare il valore per entrambi i parametri sia null.

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
```

## <a name="hybrid-modern-authentication-with-alternate-id"></a>Autenticazione moderna ibrida con ID alternativo

>[!IMPORTANT]
>Di seguito è stato testato solo con AD FS e i provider di identità non 3rd party.

### <a name="exchange-and-skype-for-business"></a>Exchange e Skype for Business
Se si usa l'id di accesso alternativo con Exchange e Skype for Business, l'esperienza dell'utente varia a seconda se si utilizza memoria alta.

>[!NOTE]
>Per risultati ottimali per l'utente finale, Microsoft consiglia di usare l'autenticazione moderna ibrida.

altre informazioni, vedere o [panoramica dell'autenticazione moderna ibrida](https://support.office.com/en-us/article/Hybrid-Modern-Authentication-overview-and-prerequisites-for-using-it-with-on-premises-Skype-for-Business-and-Exchange-servers-ef753b32-7251-4c9e-b442-1a5aec14e58d)

### <a name="pre-requisites-for-exchange-and-skype-for-business"></a>Prerequisiti per Exchange e Skype for Business
Di seguito sono prerequisiti per ottenere l'accesso SSO con un ID alternativo.

- Exchange Online deve avere attivata l'autenticazione moderna.
- Skype per Business (SFB) Online deve avere attivata l'autenticazione moderna.
- Exchange locale devono avere l'autenticazione moderna abilitata.  Lo scambio CU19 2013 o Exchange 2016 CU18 e backup è obbligatorio in tutti i server Exchange. Nessun Exchange 2010 nell'ambiente.
- Skype for Business locale deve avere l'autenticazione moderna abilitata.
- È necessario usare i client di Exchange e Skype che l'autenticazione moderna abilitata. Tutti i server devono eseguire SFB Server 2015 CU5.
- Skype per i client di Business che sono in grado di supportare l'autenticazione moderna
   - iOS, Android, Windows Phone
   - 2016 SFB (MA è impostata su ON per impostazione predefinita, ma assicurarsi che non è stato disabilitato).
   - 2013 SFB (agente di gestione è OFF per impostazione predefinita, pertanto è necessario verificare MA è stata attivata.)
   - Desktop Mac SFB
- Client di Exchange che sono in grado di supportare l'autenticazione moderna e supportano AltID regkeys
    - Office Pro Plus 2016 solo





#### <a name="supported-office-version"></a>Versione di Office supportata

Configurazione di directory per l'accesso SSO con id alternativo tramite id alternativo può causare richieste aggiuntive per l'autenticazione se queste configurazioni aggiuntive non vengono completate. Vedere l'articolo per un possibile impatto sull'esperienza utente con id alternativo.

Con la seguente configurazione aggiuntiva, l'esperienza utente risulta migliorata in modo significativo, ed è possibile ottenere quasi zero richieste di autenticazione per gli utenti di id alternativo all'interno dell'organizzazione.

##### <a name="step-1-update-to-required-office-version"></a>Passaggio 1. L'aggiornamento alla versione di office necessari
Versione di Office 1712 (non build 8827.2148) e versioni successive hanno aggiornato la logica di autenticazione per gestire lo scenario di id alternativo. Per sfruttare la nuova logica, le macchine client devono essere aggiornati alla versione di office 1712 (non build 8827.2148) e versioni successive.

##### <a name="step-2-configure-registry-for-impacted-users-using-group-policy"></a>Passaggio 2. Configurazione del Registro di sistema per gli utenti interessati usando criteri di gruppo
Le applicazioni di office si basano sulle informazioni inserite dall'amministratore di directory per identificare l'ambiente di id alternativo. Le chiavi del Registro di sistema seguenti devono essere configurate per consentire le applicazioni di office di autenticare l'utente con id alternativo senza visualizzare prompt aggiuntivi

|RegKey da aggiungere|Nome data Regkey, tipo e valore|Windows 7/8|Windows 10|Descrizione|
|-----|-----|-----|-----|-----|
|HKEY_CURRENT_USER\Software\Microsoft\AuthN|DomainHint</br>REG_SZ</br>contoso.com|Obbligatorio|Obbligatorio|Il valore di questa regkey è un nome di dominio personalizzato verificato nel tenant dell'organizzazione. Ad esempio, Contoso corp può fornire un valore di Contoso.com in questo regkey se Contoso.com è uno dei nomi di dominio personalizzato verificato nel tenant Contoso.onmicrosoft.com.|
HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Common\Identity|EnableAlternateIdSupport</br>REG_DWORD</br>1|Obbligatorio per Outlook 2016 ProPlus|Obbligatorio per Outlook 2016 ProPlus|Il valore di questa regkey può essere 1 o 0 per indicare all'applicazione Outlook se è necessario coinvolgere la logica di autenticazione migliorato id alternativo.|
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\16.0\Common\Identity|DisableADALatopWAMOverride</br>REG_DWORD</br>1|Non applicabile|Obbligatorio.|Ciò garantisce che Office non utilizza WAM come alt-id non è supportato dal Web.|
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\16.0\Common\Identity|DisableAADWAM</br>REG_DWORD</br>1|Non applicabile|Obbligatorio.|Ciò garantisce che Office non utilizza WAM come alt-id non è supportato dal Web.|
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\contoso.com\sts|&#42;</br>REG_DWORD</br>1|Obbligatorio|Obbligatorio|Questo regkey utilizzabile per impostare il servizio token di sicurezza come un'area attendibile nelle impostazioni di internet. Distribuzione di ad FS standard consiglia di aggiungere lo spazio dei nomi di ad FS all'area Intranet locale di Internet Explorer|

## <a name="new-authentication-flow-after-additional-configuration"></a>Nuovo flusso di autenticazione dopo la configurazione aggiuntiva

![Flusso di autenticazione](media/Configure-Alternate-Login-ID/alt1a.png)

1. a: È stato effettuato il provisioning utente in Azure AD usando l'id alternativo
   </br>b: Amministratore di directory effettua il push regkey necessarie impostazioni di computer client interessati
2. L'autenticazione nel computer locale e apre un'applicazione di office
3. Applicazione di Office accetta le credenziali della sessione locale
4. Applicazione di Office esegue l'autenticazione con Azure AD tramite hint di dominio inserito dall'amministratore e le credenziali locali
5. Azure AD autentica l'utente indirizzando correggere dell'area di autenticazione di federazione ed emettere un token

## <a name="applications-and-user-experience-after-the-additional-configuration"></a>Esperienza utente e le applicazioni dopo la configurazione aggiuntiva

### <a name="non-exchange-and-skype-for-business-clients"></a>Da Exchange e Skype per i client di Business
|Client|Informativa sul supporto|Note|
| ----- | -----|-----|
|Microsoft Teams|Supportato|<li>Microsoft Teams supporta AD FS (SAML-P, WS-Fed, WS-Trust e OAuth) e l'autenticazione moderna.</li><li> Core Microsoft Teams, ad esempio le funzionalità di canali, chat e i file di lavoro con ID di accesso alternativo.</li><li>app di terze parti il 1 ° e 3rd devono essere Investigate separatamente dal cliente. Questo avviene perché ogni applicazione dispone di propri protocolli di autenticazione al supporto.</li>|     
|OneDrive for Business|Supportato: chiave del Registro di sistema lato client consigliato |Con un ID alternativo configurato noterete che il nome UPN locale è già popolato nel campo della verifica. Questa operazione deve essere modificato per l'identità alternativa che è in uso. È consigliabile usare la chiave del Registro di sistema lato client indicata in questo articolo: Office 2013 e Lync 2013 richiedono periodicamente le credenziali di SharePoint Online, OneDrive e Lync Online.|
|OneDrive for Business Mobile Client|Supportato|| 
|Pagina di attivazione Office 365 Pro Plus|Supportato: chiave del Registro di sistema lato client consigliato|Con un ID alternativo configurato noterete che il nome UPN locale è già popolato nel campo della verifica. Questa operazione deve essere modificato per l'identità alternativa che è in uso. È consigliabile usare la chiave del Registro di sistema lato client indicata in questo articolo: Office 2013 e Lync 2013 richiedono periodicamente le credenziali di SharePoint Online, OneDrive e Lync Online.|

### <a name="exchange-and-skype-for-business-clients"></a>Exchange e Skype per i client di Business

|Client|Informativa sul supporto - con memoria alta|Informativa sul supporto - senza memoria alta|
| ----- |----- | ----- |
|Outlook|Istruzioni supportate, non extra|Supportato</br></br>Con **autenticazione moderna** per Exchange Online: Supportato</br></br>Con **regolare autenticazione** per Exchange Online: Supportato con le avvertenze seguenti:</br><li>Deve trovarsi in un computer appartenente al dominio e connessi alla rete aziendale </li><li>È possibile utilizzare solo ID alternativo in ambienti che non consentono l'accesso esterno per gli utenti della cassetta postale. Ciò significa che gli utenti possono autenticare solo alla propria cassetta postale in un modo supportato quando vengono connessi e aggiunta alla rete azienda, in una VPN o connesso tramite accesso diretto alle macchine, ma viene visualizzato un paio di richieste aggiuntive quando si configura il profilo di Outlook.| 
|Cartelle pubbliche ibrido|Supportato, non aggiuntive richieste.|Con **autenticazione moderna** per Exchange Online: Supportato</br></br>Con **regolare autenticazione** per Exchange Online: Non supportato</br></br><li>Cartelle pubbliche ibrido non sono in grado di espandere se gli ID alternativi vengono usati e pertanto non devono essere utilizzati oggi stesso con metodi di autenticazione regolari.|
|Cross premise delega|Vedere [configurare Exchange per supportare le autorizzazioni delegate cassetta postale in una distribuzione ibrida](https://technet.microsoft.com/library/mt784505.aspx)|Vedere [configurare Exchange per supportare le autorizzazioni delegate cassetta postale in una distribuzione ibrida](https://technet.microsoft.com/library/mt784505.aspx)|
|Accesso alla cassetta postale di archivio (cassetta postale in locale dall'archivio nel cloud)|Istruzioni supportate, non extra|Supportati: gli utenti ottengono un prompt dei comandi aggiuntivi per le credenziali quando si accede a nell'archivio, dovranno fornire l'ID alternativo quando richiesto.| 
|Outlook Web Access|Supportato|Supportato|
|Outlook App per dispositivi mobili per Android, IOS e Windows Phone|Supportato|Supportato|
|Skype for Business o Lync|Supportato, senza alcuna richiesta visualizzata extra|Supportato (tranne che come indicato), ma vi è un potenziale confusione nell'utente.</br></br>Nei client per dispositivi mobili, di un Id alternativo è supportato solo se l'indirizzo SIP = indirizzo di posta elettronica = ID alternativo.</br></br> Gli utenti potrebbe essere necessario effettuare l'accesso due volte per il Skype per Business desktop client, prima di tutto l'UPN locale e quindi utilizzando l'ID alternativo. (Si noti che il "indirizzo di accesso" è in realtà l'indirizzo SIP che potrebbe non essere lo stesso nome di"utente", anche se spesso sono). Alla prima richiesta di un nome utente, l'utente deve immettere il nome UPN, anche se si è erroneamente prepopolato con l'indirizzo SIP o un ID alternativo. Dopo che l'utente fa clic su Accedi con il nome UPN, l'utente viene nuovamente visualizzato richiesta del nome, questa volta pre-popolato con il nome UPN. Questa volta l'utente deve sostituire con l'ID alternativo e fare clic su Accedi per completare il processo di accesso. Nei client per dispositivi mobili, gli utenti devono immettere l'ID utente in locale nella pagina avanzata utilizzando il formato di stile SAM (dominio\nomeutente), non il formato UPN.</br></br>Dopo l'esito positivo accesso, se Skype for Business o Lync è indicato "Exchange richiede le credenziali", è necessario fornire le credenziali valide per la cassetta postale in cui si trova. Se la cassetta postale è nel cloud che è necessario fornire l'ID alternativo. Se la cassetta postale è locale, è necessario fornire l'UPN locale.| 
 
## <a name="additional-details--considerations"></a>Altre informazioni e considerazioni sul

-   La funzionalità di ID di accesso alternativo è disponibile per gli ambienti federativi con AD FS distribuito.  Non è supportata negli scenari seguenti:
    -   Non instradabili domini (ad esempio, contoso. Local) che non possono essere verificati da Azure AD.
    -   Gestire gli ambienti che non dispongono di AD FS distribuito.


-   Quando abilitata, la funzionalità di ID di accesso alternativo è disponibile solo per l'autenticazione di nome utente e password tra tutti i protocolli di autenticazione nome e la password utente supportati da AD FS (SAML-P, WS-Fed, WS-Trust e OAuth).


-   Quando viene eseguita l'autenticazione integrata Windows (WIA) (ad esempio, quando gli utenti tentano di accedere a un'applicazione azienda in un computer aggiunto al dominio dalla rete intranet e amministratore di AD FS ha configurato i criteri di autenticazione per l'uso WIA per la rete intranet), isused UPN per l'autenticazione. Se è stata configurata alcuna regola di attestazione per il relying party per la funzionalità di ID di accesso alternativo, è necessario assicurarsi che tali regole sono ancora valide nel caso WIA.

-   Quando abilitata, la funzionalità di ID di accesso alternativo richiede almeno un server di catalogo globale sia raggiungibile dal server per ogni foresta di account utente che AD FS supporta AD FS. Errore nel raggiungere un server di catalogo globale nella foresta di account utente comporta in ADFS, eseguire il fallback per usare l'UPN. Per impostazione predefinita tutti i controller di dominio sono i server di catalogo globale.

-   Quando abilitato, se il server AD FS rileva più di un oggetto utente con lo stesso valore di ID di accesso alternativo specificato tra tutte le foreste di account utente configurato, non l'account di accesso.

-   Quando è abilitato funzionalità di ID di accesso alternativo, AD FS tenta di autenticare l'utente finale con ID di accesso alternativo prima e quindi eseguire il fallback per usare l'UPN se non è possibile trovare un account che può essere identificato dall'ID di accesso alternativo. È consigliabile assicurarsi che non esistono Nessun scontri tra l'UPN e l'ID di accesso alternativo, se si desidera comunque supportare l'account di accesso UPN. Ad esempio, impostando uno dell'attributo mail con l'altro UPN blocca l'ad altri utenti di accedere con il suo nome UPN.

-   Se una delle foreste configurate dall'amministratore è inattivo, ADFS continua a cercare l'account utente con ID di accesso alternativo nelle altre foreste configurate. Se server AD FS rileva che un utente univoco gli oggetti nelle foreste che è effettuata la ricerca, un utente accede correttamente.

-   È inoltre possibile personalizzare la pagina di accesso di AD FS per consentire agli utenti finali alcuni hint sull'ID di accesso alternativo. È possibile farlo, aggiungere la descrizione della pagina di accesso personalizzato (per altre informazioni, vedere [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx) o personalizzazione "Accesso con account aziendale" stringa precedente campo username (per ulteriori informazioni informazioni, vedere [Advanced Customization of AD FS Sign-in Pages](https://technet.microsoft.com/library/dn636121.aspx).

-   Il nuovo tipo di attestazione che contiene il valore di ID di accesso alternativo è **http:schemas.microsoft.com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>Gli eventi e contatori delle prestazioni
Contatori delle prestazioni seguenti sono state aggiunte per misurare le prestazioni dei server AD FS, quando è abilitato l'ID di accesso alternativo:

-   Le autenticazioni Id account di accesso alternative: numero di autenticazioni eseguite usando l'ID di accesso alternativo

-   Alternative di autenticazioni Id account di accesso/Sec: numero di autenticazioni eseguite usando l'ID di accesso alternativo al secondo

-   Latenza ricerca Media per ID di accesso alternativo: Media della latenza di ricerca per gli insiemi di strutture che un amministratore ha configurato per l'ID di accesso alternativo

Di seguito sono diversi casi di errore e i corrispondente impatto sull'esperienza dell'utente Accedi con gli eventi registrati da AD FS:



**Casi di errore**|**Impatto sull'esperienza di accesso**|**Event**|
---------|---------|---------
Non è possibile ottenere un valore per l'attributo SAMAccountName per l'oggetto utente|Errore di accesso|Evento con ID 364 con il messaggio di eccezione MSIS8012: Impossibile trovare l'attributo samAccountName per l'utente: '{0}'.|
L'attributo CanonicalName non accessibile|Errore di accesso|Evento con ID 364 con il messaggio di eccezione MSIS8013: CanonicalName: '{0}' dell'utente:'{1}' è in formato non corretto.|
Nelle foreste uno vengono trovati più oggetti utente|Errore di accesso|Evento con ID 364 con il messaggio di eccezione MSIS8015: Trovati più account utente con identità '{0}'nella foresta'{1}' con le identità: {2}|
Vengono trovati più oggetti utente in più foreste|Errore di accesso|Evento con ID 364 con il messaggio di eccezione MSIS8014: Trovati più account utente con identità '{0}' negli insiemi di strutture: {1}|

## <a name="see-also"></a>Vedere anche
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)


