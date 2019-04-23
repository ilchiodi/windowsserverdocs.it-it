---
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: Configurare AD FS per l'autenticazione di utenti memorizzati nelle directory LDAP
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05f8b8991e664a84c3f2b3200de4068af8d1476a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846612"
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories"></a>Configurare AD FS per l'autenticazione di utenti memorizzati nelle directory LDAP

>Si applica a: Windows Server 2016

L'argomento seguente descrive la configurazione necessaria per abilitare l'infrastruttura AD FS autenticare gli utenti la cui identità sono memorizzati nelle directory v3 conforme a Lightweight Directory Access Protocol (LDAP).

In molte organizzazioni, soluzioni di gestione delle identità è costituita da una combinazione di Active Directory, AD LDS o directory LDAP di terze parti. Con l'aggiunta del supporto di AD FS per l'autenticazione degli utenti memorizzati nelle directory v3 conforme a LDAP, è possibile trarre vantaggio dall'intera aziendale ADFS set indipendentemente dal fatto in cui sono archiviate le identità utente di funzionalità. ADFS supporta qualsiasi directory v3 conforme a LDAP.

> [!NOTE]
> Alcune delle funzionalità di ADFS includono single sign-on (SSO), l'autenticazione del dispositivo, i criteri di accesso condizionale flessibile, il supporto per da-ovunque ti trovi tramite l'integrazione con il Proxy applicazione Web e facile la federazione con Azure AD che a sua volta Consente di e gli utenti del cloud, tra cui Office 365 e ad altre applicazioni SaaS.  Per altre informazioni, vedere [Panoramica di Active Directory Federation Services](../../ad-fs/AD-FS-2016-Overview.md).

In ordine per AD FS autenticare gli utenti da una directory LDAP, è necessario connettersi questa directory LDAP per la farm AD FS mediante la creazione di un **trust del provider di attestazioni locale**.  Un trust del provider di attestazioni locale è un oggetto trust che rappresenta una directory LDAP della farm AD FS. Una variabile locale di attestazioni trust del provider di oggetti è costituito da diversi identificatori, nomi e regole che identificano la directory LDAP per il servizio federativo locale.

È possibile supportare più directory LDAP, ognuno con la propria configurazione, all'interno della stessa farm di AD FS aggiungendo più **attendibilità del provider di attestazioni locale**. Inoltre, le foreste di Active Directory Domain Services che non è attendibile per la foresta che ADFS si trova in possono essere modellate come provider di attestazioni locale. È possibile creare provider di attestazioni locale tramite Windows PowerShell.

Directory LDAP (attendibilità del provider di attestazioni locale) può coesistere con le directory di Active Directory (attendibilità del provider di attestazioni) nello stesso server AD FS, all'interno della stessa farm di ADFS, pertanto, non è in grado di autenticare e autorizzare l'accesso per gli utenti che sono una singola istanza di AD FS archiviato in entrambi AD e non AD Directory.

Solo l'autenticazione basata su form è supportata per l'autenticazione degli utenti dalla directory LDAP. L'autenticazione Windows integrata e basata su certificati non sono supportati per l'autenticazione degli utenti nelle directory LDAP.

Tutti i protocolli di autorizzazione passivo che sono supportati da AD FS, tra cui SAML, WS-Federation e OAuth sono supportate anche per le identità che vengono archiviate nelle directory LDAP.

Il protocollo di autorizzazione attivi WS-Trust supportato anche per le identità che vengono archiviate nelle directory LDAP.

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>Configurare AD FS per autenticare gli utenti archiviati in una directory LDAP
Per configurare la farm AD FS per autenticare gli utenti da una directory LDAP, è possibile completare la procedura seguente:

1.  Innanzitutto, configurare una connessione alla directory LDAP con i **New-AdfsLdapServerConnection** cmdlet:

    ```
    $DirectoryCred = Get-Credential
    $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
    ```

    > [!NOTE]
    > È consigliabile creare un nuovo oggetto di connessione per ogni server LDAP che si desidera connettersi. ADFS può connettersi a più server LDAP di replica e automaticamente il failover nel caso in cui uno specifico server LDAP è inattivo. Per questo caso, è possibile creare uno AdfsLdapServerConnection per ciascuno di questi server LDAP di replica e quindi aggiungere la matrice di oggetti di connessione usando l'opzione -**LdapServerConnection** parametro del  **Aggiungere-AdfsLocalClaimsProviderTrust** cmdlet.

    **NOTA:** Il tentativo di usare Get-Credential e digitare un nome distinto e una password da usare per l'associazione a un'istanza LDAP può comportare un errore perché del requisito di interfaccia utente per specifici formati di input, ad esempio DOMINIO\nome utente o user@domain.tld. È invece possibile usare il cmdlet ConvertTo-SecureString come indicato di seguito (l'esempio seguente presuppone l'uid = admin, ou = system come DN delle credenziali da utilizzare per eseguire l'associazione all'istanza di LDAP):

    ```
    $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
    $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
    ```

    Quindi immettere la password per l'uid = admin e completare il resto dei passaggi.

2.  Successivamente, è possibile eseguire il passaggio facoltativo di mapping degli attributi LDAP per le attestazioni di ADFS esistente usando il **New-AdfsLdapAttributeToClaimMapping** cmdlet. Nell'esempio seguente, si esegue il mapping givenName, cognome, e gli attributi LDAP CommonName per le attestazioni di AD FS:

    ```
    #Map given name claim
    $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
    # Map surname claim
    $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
    # Map common name claim
    $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
    ```

    Questo mapping viene eseguito per liberare gli attributi dall'archivio LDAP come attestazioni in ADFS per creare regole di controllo di accesso condizionale in AD FS. Consente inoltre AD FS da usare con gli schemi personalizzati negli archivi LDAP, che fornisce un modo semplice per eseguire il mapping di attributi LDAP ai reclami.

3.  Infine, è necessario registrare l'archivio LDAP con AD FS come una variabile locale di attestazioni provider attendibilità tramite il **Add-AdfsLocalClaimsProviderTrust** cmdlet:

    ```
    Add-AdfsLocalClaimsProviderTrust -Name "Vendors" -Identifier "urn:vendors" -Type Ldap

    # Connection info
    -LdapServerConnection $vendorDirectory 

    # How to locate user objects in directory
    -UserObjectClass inetOrgPerson -UserContainer "CN=VendorsContainer,CN=VendorsPartition" -LdapAuthenticationMethod Basic 

    # Claims for authenticated users
    -AnchorClaimLdapAttribute mail -AnchorClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -LdapAttributeToClaimMapping @($GivenName, $Surname, $CommonName) 

    # General claims provider properties
    -AcceptanceTransformRules "c:[Type != ''] => issue(claim=c);" -Enabled $true 

    # Optional - supply user name suffix if you want to use Ws-Trust
    -OrganizationalAccountSuffix "vendors.contoso.com"

    ```

    Nell'esempio precedente, si crea un trust del provider di attestazioni locale denominato "Fornitori". Si specificano le informazioni di connessione per AD FS per la connessione alla directory LDAP questa attendibilità del provider di attestazioni locale rappresenta assegnando `$vendorDirectory` per il `-LdapServerConnection` parametro. Si noti che nel passaggio 1, è stata assegnata `$vendorDirectory` una stringa di connessione da utilizzare quando ci si connette alla directory LDAP specifica. Infine, si specifica che il `$GivenName`, `$Surname`, e `$CommonName` attributi LDAP (che è stata associata alle attestazioni AD FS) devono essere utilizzati per il controllo di accesso condizionale, compresi i criteri di autenticazione a più fattori e rilascio regole di autorizzazione, oltre a quella di rilascio tramite attestazioni nei token di sicurezza emesso da ADFS di Active Directory. Per usare active protocolli come Ws-Trust con ADFS, è necessario specificare il parametro OrganizationalAccountSuffix, che consente di ADFS evitare ambiguità tra trust di provider di attestazioni locale durante la manutenzione di una richiesta di autorizzazione attivi.

## <a name="see-also"></a>Vedere anche
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)


