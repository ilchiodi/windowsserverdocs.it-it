---
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: Configurare ADFS per l'autenticazione degli utenti memorizzati nelle directory LDAP
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05f8b8991e664a84c3f2b3200de4068af8d1476a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories"></a>Configurare ADFS per l'autenticazione degli utenti memorizzati nelle directory LDAP

>Si applica a: Windows Server 2016

L'argomento seguente descrive la configurazione necessaria per abilitare l'infrastruttura di AD FS autenticare gli utenti la cui identità sono memorizzati nelle directory v3 conforme Lightweight Directory Access Protocol (LDAP).

In molte organizzazioni le soluzioni di gestione di identità sono costituite da una combinazione di Active Directory, AD LDS o directory LDAP di terze parti. Con l'aggiunta del supporto di ADFS per l'autenticazione degli utenti memorizzati nelle directory v3 conforme a LDAP, è possibile trarre vantaggio dall'intero aziendale ADFS insieme di funzionalità di indipendentemente in cui sono archiviate le identità utente. ADFS supporta qualsiasi directory v3 conforme a LDAP.

> [!NOTE]
> Alcune delle funzionalità di ADFS includono il single sign-on (SSO), l'autenticazione del dispositivo, criteri di accesso condizionale flessibile, il supporto per il lavoro da-ovunque tramite l'integrazione con il Proxy dell'applicazione Web e trasparente federazione con Azure AD che a sua volta consente e gli utenti del cloud, tra cui Office 365 e altre applicazioni SaaS.  Per ulteriori informazioni, vedere [Panoramica di Active Directory Federation Services ](../../ad-fs/AD-FS-2016-Overview.md).

Nell'ordine per AD FS autenticare gli utenti da una directory LDAP, è necessario connettere la directory LDAP per la farm ADFS creando un **trust del provider di attestazioni locale **.  Un trust del provider di attestazioni locale è un oggetto trust che rappresenta una directory LDAP nella farm ADFS. Oggetto è costituito da diversi identificatori, nomi e regole che identificano la directory LDAP al servizio federativo locale trust del provider di attestazioni locale.

È possibile supportare più directory LDAP, ognuno con la sua configurazione, all'interno della stessa farm AD FS aggiungendo più **attendibilità del provider di attestazioni locale **. Inoltre, gli insiemi di strutture di dominio Active Directory che non è considerato attendibile dalla foresta che ADFS vive in possono essere modellate anche come provider di attestazioni locale. È possibile creare provider di attestazioni locale tramite Windows PowerShell.

La directory LDAP (attendibilità del provider di attestazioni locale) può coesistere con le directory di Active Directory (attendibilità del provider di attestazioni) nello stesso server ADFS, all'interno della stessa farm AD FS, di conseguenza, una singola istanza di AD FS è in grado di autenticazione e autorizzazione dell'accesso per gli utenti che sono archiviati sia in Active Directory e non Active Directory.

Solo l'autenticazione basata su form è supportata per l'autenticazione degli utenti dalla directory LDAP. L'autenticazione basata sui certificati e integrata Windows non sono supportati per l'autenticazione degli utenti nelle directory LDAP.

Tutti i protocolli di autorizzazione passivo supportati da AD FS, tra cui SAML, WS-Federation e OAuth sono supportati anche per le identità archiviati nelle directory LDAP.

Il protocollo di autorizzazione attivo WS-Trust è supportato anche per le identità archiviati nelle directory LDAP.

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>Configurare ADFS per autenticare gli utenti archiviati in una directory LDAP
Per configurare la farm di ADFS per autenticare gli utenti da una directory LDAP, è possibile completare i passaggi seguenti:

1.  Prima di tutto, configurare una connessione per la directory LDAP utilizzando il **New-AdfsLdapServerConnection** cmdlet:

    ```
    $DirectoryCred = Get-Credential
    $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
    ```

    > [!NOTE]
    > È consigliabile creare un nuovo oggetto di connessione per ogni server LDAP che si desidera connettersi. ADFS può connettersi a più server di replica LDAP e automaticamente il failover nel caso in cui un server LDAP specifico è inattivo. Per questo caso, è possibile creare uno AdfsLdapServerConnection per ciascuno di questi server LDAP di replica e quindi aggiungere la matrice di oggetti di connessione tramite-**LdapServerConnection** parametro del **Aggiungi AdfsLocalClaimsProviderTrust** cmdlet.

    **Nota:** il tentativo di utilizzare Get-Credential e digitare un nome distinto e una password da utilizzare per eseguire il binding a un'istanza LDAP potrebbe causare un errore perché del requisito dell'interfaccia utente per i formati di input specifici, ad esempio DOMINIO\nome utente o user@domain.tld. È invece possibile utilizzare il cmdlet ConvertTo-SecureString come indicato di seguito (l'esempio seguente presuppone uid = admin, ou = system, come il nome distinto delle credenziali da utilizzare per eseguire il binding all'istanza di LDAP):

    ```
    $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
    $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
    ```

    Quindi immettere la password per l'uid = admin e completare il resto della procedura.

2.  Successivamente, è possibile eseguire il passaggio facoltativo di mapping di attributi LDAP per le attestazioni di ADFS esistente utilizzando il **New-AdfsLdapAttributeToClaimMapping** cmdlet. Nell'esempio seguente, si esegue il mapping givenName, cognome, e CommonName LDAP gli attributi per le attestazioni di ADFS:

    ```
    #Map given name claim
    $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
    # Map surname claim
    $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
    # Map common name claim
    $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
    ```

    Questo mapping viene eseguito per rendere disponibile come attestazioni in ADFS per creare le regole di controllo di accesso condizionale in AD FS attributi dall'archivio LDAP. Consente inoltre di ADFS lavorare con gli schemi personalizzati in archivi LDAP, fornendo un modo semplice per eseguire il mapping di attributi LDAP alle attestazioni.

3.  Infine, è necessario registrare l'archivio LDAP con AD FS come locale attestazioni provider attendibilità utilizzando il **Aggiungi AdfsLocalClaimsProviderTrust** cmdlet:

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

    Nell'esempio precedente, si sta creando un trust del provider di attestazioni locale denominato "Fornitori". Si immettono informazioni di connessione per AD FS per connettersi alla directory LDAP questa attendibilità del provider di attestazioni locale rappresenta assegnando `$vendorDirectory`per il `-LdapServerConnection`parametro. Si noti che nel passaggio 1, è stata assegnata `$vendorDirectory`una stringa di connessione da utilizzare durante la connessione alla directory LDAP specifiche. Infine, si specifica che il `$GivenName`, `$Surname`, e `$CommonName`attributi LDAP (che è stata mappata per le attestazioni di AD FS) devono essere utilizzati per il controllo di accesso condizionale, compresi i criteri di autenticazione a più fattori e regole di autorizzazione rilascio, oltre che per il rilascio tramite attestazioni nei token di sicurezza emesso da ADFS di Active Directory. Per poter usare protocolli attivi come Ws-Trust con ADFS, è necessario specificare il parametro OrganizationalAccountSuffix, che consente di ADFS evitare ambiguità tra attendibilità del provider di attestazioni locale quando una richiesta di autorizzazione active di manutenzione.

## <a name="see-also"></a>Vedere anche
[Operazioni di ADFS](../../ad-fs/AD-FS-2016-Operations.md)


