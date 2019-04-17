---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: Configurazione di ID di accesso alternativo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/04/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb4c98ff1090a9d3c35654614a43e12db99d691d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-alternate-login-id"></a>Configurazione di ID di accesso alternativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Gli utenti possono accedere alle applicazioni di Active Directory Federation Services (ADFS) abilitata utilizzando qualsiasi forma di identificatore utente accettato dai servizi di dominio Active Directory (AD DS). Sono inclusi i nomi dell'entità utente (UPN) (johndoe@contoso.com) o di dominio completo di nomi di account sam (contoso\johndoe o contoso.com\johndoe #).

In alcuni ambienti, a causa di criteri aziendali o le dipendenze dell'applicazione line-of-business in locale, gli utenti finali potrebbe essere presente il messaggio di posta elettronica e non le UPN nome o l'indirizzo account sam. In alcuni casi, il nome UPN è anche non instradabile (jdoe@contoso.local) e viene utilizzato solo per l'autenticazione in applicazioni nella rete aziendale.

A partire da non instradabile ogni dominio (ad esempio. Proprietà di Contoso. Local) non possono essere verificate, Office 365 richiede tutti gli ID di accesso per essere instradabile su internet. Se il nome UPN locale viene utilizzato un dominio non instradabile (ad esempio. Contoso. Local), o il nome UPN esistente non può essere modificato a causa di dipendenze dell'applicazione locale, è consigliabile configurare l'ID di accesso alternativo. ID di accesso alternativo consente di configurare un registro esperienza in cui gli utenti possono accedere con un attributo diverso da loro UPN, ad esempio posta elettronica.

Uno dei vantaggi di questa funzionalità è che consente di adottare SaaS provider, ad esempio Office 365 senza modificare l'UPN locale. Consente inoltre di supportare le applicazioni line-of-business servizio con le identità di provisioning per gli utenti.

> [!IMPORTANT]
> Con ID alternativo in ambienti ibridi con Exchange e/o di Skype for Business è supportato, ma non è consigliata. Utilizzando lo stesso set di credenziali (ad esempio, il nome UPN) per in locale e online offre la migliore esperienza utente in un ambiente ibrido.  Microsoft consiglia ai clienti modificare i nomi UPN se possibile per evitare la necessità di ID alternativo. Con ID alternativo con Lync o Skype for business richiede Lync Server 2013 o versione successiva. I clienti che utilizzano l'ID alternativo è consigliabile attivare [l'autenticazione moderna](https://support.office.com/article/using-office-365-modern-authentication-with-office-clients-776c0036-66fd-41cb-8928-5495c0f9168a) per Exchange in Office 365 per un'esperienza utente migliorata. Inoltre, i clienti che usano Skype for Business con i client per dispositivi mobili è necessario assicurarsi che l'indirizzo SIP è identico all'indirizzo di posta elettronica dell'utente (e ID alternativo). 

Fare riferimento alla tabella di seguito per l'esperienza utente con ID alternativo vari client Office 365 con autenticazione normale, l'autenticazione moderna e l'autenticazione basata su certificato (è necessario abilitare l'autenticazione moderna).

|**Tipi di client**|**Informazioni aggiuntive**|**Supporto istruzione - autenticazione normale e moderna**|**Descrizione**|
|--------------------|------------------------------|------------------------------|------------------------------|
|Outlook|Autenticazione normale: Deve essere su un computer aggiunto al dominio e connessi alla rete aziendale<br /><br />L'autenticazione moderna: supportati|È possibile utilizzare solo ID alternativo in ambienti che non consentono l'accesso esterno per gli utenti della cassetta postale. Ciò significa che gli utenti possono solo eseguire l'autenticazione alla cassetta postale sono in modo supportato quando sono connessi e aggiunto alla rete aziendale, in una VPN o connesso tramite accesso diretto. Se si decide di configurare l'autenticazione moderna (noto come ADAL) è possibile utilizzare Outlook da computer appartenenti a un connesso non appartenenti al dominio, ma si otterrà un paio di aggiuntive richieste quando si configura il profilo di Outlook.<br /><br />Vedere la prima immagine sotto la tabella per demo esperienza utente.|[Autenticazione moderna in Office 2013](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|Cartelle pubbliche ibrida|Autenticazione normale: Non è supportata<br /><br />L'autenticazione moderna: supportati|Ibrida delle cartelle pubbliche non sarà in grado di espandere se alternativo ID vengono usati e pertanto non deve essere utilizzato oggi con metodi di autenticazione normale. Se si desidera essere in grado di utilizzare cartelle pubbliche ibride è necessario configurare l'autenticazione moderna (noto come ADAL).<br /><br />Vedere la prima immagine sotto la tabella per demo esperienza utente.|[Autenticazione moderna in Office 2013](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|Tra locale delega|Non è supportato|Attualmente tra le autorizzazioni non sono supportate in una configurazione ibrida locale, ma anche non funzioneranno se si utilizza AltID.||
|Accesso alla cassetta postale archivio (cassetta postale locale - archiviazione nel cloud)|È supportato|Gli utenti ricevono una richiesta aggiuntiva per le credenziali quando si accede l'archivio, è necessario fornire alternative sono ID quando richiesto.<br /><br />Vedere la prima immagine sotto la tabella per demo esperienza utente.||
|Pagina di Office 365 Pro Plus attivazione|Supportato - consigliato chiave del Registro di sistema lato client|Con ID alternativo configurato verrà visualizzato che il nome UPN locale viene prepopolato nel campo verifica. Questa operazione deve essere modificati per l'identità alternativo che viene utilizzato. Si consiglia di utilizzare la chiave di registro sul lato client indicata nella colonna del collegamento.<br /><br />Vedere la seconda immagine sotto la tabella per demo esperienza utente.|[Office 2013 e Lync 2013 periodicamente vengono richieste le credenziali di SharePoint Online, OneDrive e Lync Online](https://support.microsoft.com/en-us/kb/2913639)|
|Microsoft Teams|È supportato|Microsoft Teams supports AD FS (SAML-P, WS-Fed, WS-Trust, and OAuth) and Modern Authentication.<br/><br/> Core Microsoft Teams such as Channels, chats and files functionalities does work with Altnernate Login ID. </br></br>1st and 3rd party apps have to be separately investigated  by the customer. This is because each application has their own supportability authentication protocols.| 
|Skype for Business o Lync|Supportato (eccetto annotata) ma esiste un rischio per la confusione.  Nei computer client per dispositivi mobili, Id alternativo è supportato solo se l'indirizzo SIP = indirizzo di posta elettronica = ID alternativo.|Gli utenti potrebbe essere necessario effettuare l'accesso due volte per il Skype per Business desktop client, prima di tutto il nome UPN locale e quindi utilizzando l'ID alternativo. (Si noti che il "indirizzo di accesso" è effettivamente l'indirizzo SIP che potrebbe non essere lo stesso nome di"utente", anche se è spesso).  Quando richiesto prima di un nome utente, l'utente deve immettere il nome UPN, anche se si è erroneamente precompilare con l'indirizzo SIP o ID alternativo. Dopo che l'utente fa clic sull'accesso in ingresso con il nome UPN, verranno nuovamente visualizzati prompt dei comandi di nome utente, questa volta popolato automaticamente con il nome UPN. Questa volta, è necessario sostituire con l'ID alternativo e fare clic su Accedi a completare il processo di accesso. Nei computer client per dispositivi mobili, gli utenti devono immettere l'ID utente locale nella pagina avanzata, usando il formato di stile SAM (DOMINIO\nome utente), non il formato UPN.<br /><br />Dopo l'esito positivo accesso, se Skype per Business o Lync è indicato "Exchange è necessario fornire le credenziali", è necessario fornire le credenziali valide per la cassetta postale in cui si trova. Se la cassetta postale è nel cloud che è necessario fornire l'ID alternativo. Se la cassetta postale è locale che è necessario fornire il nome UPN locale.|[Autenticazione moderna in Office 2013](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|Outlook Web Access|È supportato|||
|Outlook Mobile App per Android, IOS e Windows Phone|È supportato|||
|OneDrive for Business|Supportato - consigliato chiave del Registro di sistema lato client|Con ID alternativo configurato verrà visualizzato che il nome UPN locale viene prepopolato nel campo verifica. Questa operazione deve essere modificati per l'identità alternativo che viene utilizzato. Si consiglia di utilizzare la chiave di registro sul lato client indicata nella colonna del collegamento.<br /><br />Vedere la seconda immagine sotto la tabella per demo esperienza utente.|[Office 2013 e Lync 2013 periodicamente vengono richieste le credenziali di SharePoint Online, OneDrive e Lync Online](https://support.microsoft.com/en-us/kb/2913639)|
|OneDrive for Business Client per dispositivi mobili|È supportato|||

![Accesso alternativo](media/Configure-Alternate-Login-ID/ADFS_Alt_ID1.png)

![Accesso alternativo](media/Configure-Alternate-Login-ID/ADFS_Alt_ID2.png)

![Accesso alternativo](media/Configure-Alternate-Login-ID/ADFS_Alt_ID3.png)

Di seguito, gli screenshot seguenti sono un altro esempio con Skype for Business.  Nell'esempio viene utilizzate le informazioni seguenti


- SIP:userA@contoso.com 
- UPN:userA@contoso.local
- Posta elettronica:userA@contoso.com
- AltId:userA@contoso.com 

Immettere l'indirizzo SIP nel campo di accesso.

![Skype](media/Configure-Alternate-Login-ID/SkypeA.png)

![Skype](media/Configure-Alternate-Login-ID/SkypeB.png)

![Skype](media/Configure-Alternate-Login-ID/SkypeC.png)

## <a name="to-configure-alternate-login-id"></a>Per configurare l'ID di accesso alternativo
Per configurare l'ID di accesso alternativo, è necessario eseguire le attività seguenti:

Configurare il provider di attestazioni di ADFS per abilitare l'ID di accesso alternativo

1.  Install [KB2919355](https://go.microsoft.com/fwlink/?LinkID=396590).  Puoi ottenere lo strumento tramite Windows Update Services o scaricarlo direttamente.

2.  Aggiornare la configurazione di ADFS eseguendo il seguente cmdlet PowerShell in uno qualsiasi dei server federativi della farm (se si dispone di una farm database interno di Windows, è necessario eseguire questo comando sul server ADFS primario della farm):

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>

    ```

    **AlternateLoginID** è il nome LDAP dell'attributo che si desidera utilizzare per l'accesso.

    **LookupForests** è l'elenco di foresta che gli utenti appartengono a DNS.

    Per abilitare la funzionalità di ID di accesso alternativo, è necessario configurare i parametri - AlternateLoginID sia - LookupForests con un valore non null, valido.

    Nell'esempio seguente, si attiva la funzionalità di ID di accesso alternativo in modo che possono accedere gli utenti con account contoso.com e fabrikam.com foreste per AD FS applicazioni abilitate all'uso con l'attributo "stampa".

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
    ```

3.  Per disabilitare questa funzionalità, impostare il valore per entrambi i parametri possono essere null.

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
    ```

4.  Per abilitare l'ID di accesso alternativo con Azure AD, non configurazioni aggiuntive sono necessari passaggi quando si usa Azure AD Connect.   È possibile configurare ID alternativo direttamente dalla procedura guidata.  Vedere in modo univoco che identifica gli utenti della sezione [Connetti ad Azure AD](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-get-started-custom/#connect-to-azure-ad).

## <a name="additional-details--considerations"></a>Ulteriori informazioni e considerazioni sul

-   La funzionalità di ID di accesso alternativo è disponibile solo per ambienti federati con ADFS distribuito.  Non è supportato nei seguenti scenari:
    -   Non instradabile domini (ad esempio, contoso. Local) che non possono essere verificati da Azure AD.
    -   Gestire gli ambienti che non dispongono di AD FS distribuito.


-   Quando abilitata, la funzionalità di ID di accesso alternativo è disponibile solo per l'autenticazione nome utente/password tra tutti gli utente/password nome protocolli di autenticazione supportati da AD FS (SAML-P, WS-Fed, WS-Trust e OAuth).


-   Quando viene eseguita l'autenticazione integrata Windows (WIA) (ad esempio, quando gli utenti tentano di accedere a un'applicazione azienda su un computer aggiunto al dominio dalla rete intranet e AD FS amministratore ha configurato Criteri di autenticazione per l'utilizzo di WIA per la rete intranet), verrà utilizzato per l'autenticazione UPN. Se è stata configurata eventuali regole attestazione per le relying party per la funzionalità di ID di accesso alternativo, assicurarsi che tali regole sono ancora valide nel caso WIA.

-   Quando abilitata, la funzionalità di ID di accesso alternativo richiede almeno un server di catalogo globale sia raggiungibile dal server AD FS per ogni foresta di account utente che supporta AD FS. Errore di raggiungere un server di catalogo globale nella foresta degli account utente comporterà in ADFS, eseguire il fallback per l'utilizzo di UPN. Per impostazione predefinita tutti i controller di dominio sono server di catalogo globale.

-   Quando abilitata, se il server AD FS rileva più di un oggetto utente con lo stesso valore di ID di accesso alternativo specificato in tutte le foreste di account utente configurato, l'account di accesso avrà esito negativo.

-   Quando è abilitata la funzionalità di ID di accesso alternativo, AD FS tenterà di autenticare l'utente finale con ID di accesso alternativo prima e quindi eseguire il fallback per l'utilizzo di UPN se non trova un account che è possibile identificare l'ID di accesso alternativo. Assicurarsi che non esistono conflitti tra ID di accesso alternativo e il nome UPN, se si desidera supportano ancora l'account di accesso UPN. Ad esempio, attributo mail impostazione uno con l'altra UPN blocca l'altro utente dall'accesso con il suo nome UPN.

-   Se una delle foreste configurato dall'amministratore è inattivo, ADFS continuerà a cercare l'account utente con un ID di accesso alternativo in altre foreste configurate. Se il server AD FS rileva che un utente univoco oggetti nelle foreste che è effettuata la ricerca, un utente accederà correttamente.

-   Si consiglia inoltre di personalizzare la pagina di accesso di ADFS per fornire agli utenti finali alcuni suggerimento per l'ID di accesso alternativo. È possibile farlo aggiungendo la descrizione della pagina di accesso personalizzate (per ulteriori informazioni, vedere [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx) o la personalizzazione di "accesso con account dell'organizzazione" stringa sopra il campo nome utente (per ulteriori informazioni, vedere [Advanced Customization of AD FS Sign-in Pages](https://technet.microsoft.com/library/dn636121.aspx).

-   Il nuovo tipo di attestazione che contiene il valore di ID di accesso alternativo **http:schemas.microsoft.com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>Gli eventi e contatori delle prestazioni
Sono stati aggiunti i seguenti contatori delle prestazioni per misurare le prestazioni dei server ADFS quando è abilitato l'ID di accesso alternativo:

-   Le autenticazioni con Id account di accesso alternative: numero di autenticazioni eseguita utilizzando l'ID di accesso alternativo

-   Alternare le autenticazioni al secondo Id di accesso: numero di autenticazioni eseguite usando un ID di accesso alternativo al secondo

-   Media di latenza di ricerca per ID di accesso alternativo: latenza di ricerca Media nelle foreste che un amministratore ha configurato per l'ID di accesso alternativo

Di seguito sono vari casi di errore e corrispondente impatto sull'esperienza accesso dell'utente con gli eventi registrati da AD FS:



**Casi di errore**|**Impatto sull'esperienza di accesso**|**Evento**|
---------|---------|---------
Impossibile ottenere un valore per SAMAccountName dell'oggetto utente|Errore di accesso|ID evento 364 con il messaggio di eccezione MSIS8012: Impossibile trovare samAccountName dell'utente: '{0}'.|
L'attributo CanonicalName non è accessibile|Errore di accesso|ID evento 364 con il messaggio di eccezione MSIS8013: CanonicalName: '{0}' dell'utente: '{1}' è in formato non valido.|
Più oggetti utente che si trovano in foreste uno|Errore di accesso|ID evento 364 con il messaggio di eccezione MSIS8015: trovati più account utente con identità: '{0}' nella foresta '{1}' con identità: {2}|
Più oggetti utente che si trovano in più foreste|Errore di accesso|ID evento 364 con il messaggio di eccezione MSIS8014: trovati più account utente con identità: '{0}' in insiemi di strutture: {1}|

## <a name="see-also"></a>Vedere anche
[Operazioni di ADFS](../../ad-fs/AD-FS-2016-Operations.md)


