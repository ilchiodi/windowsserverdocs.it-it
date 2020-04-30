---
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: Configurare AD FS per l'autenticazione di utenti memorizzati nelle directory LDAP
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a3e429d43fd644cd2b8ba3a5b123deecc2696f24
ms.sourcegitcommit: 912a5a402ecc6b39c1584338ea635a2ac11a4eb9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2020
ms.locfileid: "82219285"
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories-in-windows-server-2016-or-later"></a>Configurare AD FS per autenticare gli utenti archiviati in directory LDAP in Windows Server 2016 o versioni successive

Nell'argomento seguente viene descritta la configurazione necessaria per abilitare l'infrastruttura di AD FS per l'autenticazione degli utenti le cui identità sono archiviate nelle directory conformi a LDAP (Lightweight Directory Access Protocol) v3.

In molte organizzazioni, le soluzioni di gestione delle identità sono costituite da una combinazione di Active Directory, AD LDS o directory LDAP di terze parti. Con l'aggiunta del supporto AD FS per l'autenticazione degli utenti archiviati in directory conformi a LDAP v3, è possibile trarre vantaggio dall'intero set di funzionalità di AD FS di livello aziendale indipendentemente dalla posizione in cui sono archiviate le identità utente. AD FS supporta qualsiasi directory conforme a LDAP v3.

> [!NOTE]
> Alcune delle funzionalità di AD FS includono Single Sign-On (SSO), l'autenticazione del dispositivo, criteri flessibili di accesso condizionale, il supporto per il lavoro da qualsiasi luogo attraverso l'integrazione con il proxy dell'applicazione Web e la Federazione trasparente con Azure AD che a sua volta consente all'utente e agli utenti di usare il cloud, tra cui Office 365 e altre applicazioni SaaS.  Per ulteriori informazioni, vedere [Active Directory Federation Services Overview](../../ad-fs/AD-FS-2016-Overview.md).

Per consentire AD FS di autenticare gli utenti da una directory LDAP, è necessario connettere questa directory LDAP alla farm di AD FS creando un **trust del provider di attestazioni locale**.  Un trust del provider di attestazioni locale è un oggetto trust che rappresenta una directory LDAP nella farm AD FS. Un oggetto trust del provider di attestazioni locale è costituito da diversi identificatori, nomi e regole che identificano questa directory LDAP per il servizio federativo locale.

È possibile supportare più directory LDAP, ognuna con la propria configurazione, all'interno della stessa farm AD FS aggiungendo più **trust del provider di attestazioni locali**. Inoltre, le foreste di Active Directory Domain Services non ritenuti attendibili dalla foresta in cui risiede AD FS possono anche essere modellate come trust del provider di attestazioni locali. È possibile creare trust del provider di attestazioni locale usando Windows PowerShell.

Le directory LDAP (trust del provider di attestazioni locali) possono coesistere con le directory di AD (trust del provider di attestazioni) nello stesso server AD FS, all'interno della stessa farm AD FS, pertanto, una singola istanza di AD FS è in grado di autenticare e autorizzare l'accesso per gli utenti archiviati in directory e non AD.

Per l'autenticazione degli utenti dalle directory LDAP è supportata solo l'autenticazione basata su form. L'autenticazione di Windows integrata e basata su certificati non è supportata per l'autenticazione degli utenti nelle directory LDAP.

Tutti i protocolli di autorizzazione passiva supportati da AD FS, tra cui SAML, WS-Federation e OAuth, sono supportati anche per le identità archiviate nelle directory LDAP.

Il protocollo di autorizzazione attiva WS-Trust è supportato anche per le identità archiviate nelle directory LDAP.

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>Configurare AD FS per autenticare gli utenti archiviati in una directory LDAP
Per configurare la farm AD FS per autenticare gli utenti da una directory LDAP, è possibile completare i passaggi seguenti:

1. Per prima cosa, configurare una connessione alla directory LDAP usando il cmdlet **New-AdfsLdapServerConnection** :

   ```
   $DirectoryCred = Get-Credential
   $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
   ```

   > [!NOTE]
   > Si consiglia di creare un nuovo oggetto connessione per ogni server LDAP a cui si desidera connettersi. AD FS possibile connettersi a più server LDAP di replica ed eseguire automaticamente il failover nel caso in cui un server LDAP specifico non sia attivo. In tal caso, è possibile creare un AdfsLdapServerConnection per ognuno di questi server LDAP di replica e quindi aggiungere la matrice di oggetti connessione utilizzando il parametro-**LdapServerConnection** del cmdlet **Add-AdfsLocalClaimsProviderTrust** .

   **Nota:** Il tentativo di usare Get-Credential e digitare un DN e una password da usare per l'associazione a un'istanza LDAP potrebbe causare un errore a causa del requisito dell'interfaccia utente per formati di input specifici, ad esempio dominio\nomeutente o user@domain.tld. È invece possibile usare il cmdlet ConvertTo-SecureString come indicato di seguito (nell'esempio seguente si presuppone UID = admin, ou = System come DN delle credenziali da usare per l'associazione all'istanza LDAP):

   ```
   $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
   $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
   ```

   Immettere quindi la password per uid = Admin e completare i passaggi rimanenti.

2. Successivamente, è possibile eseguire il passaggio facoltativo del mapping degli attributi LDAP alle attestazioni di AD FS esistenti usando il cmdlet **New-AdfsLdapAttributeToClaimMapping** . Nell'esempio seguente viene mappato gli attributi DATANAME, cognome e CommonName LDAP alle attestazioni AD FS:

   ```
   #Map given name claim
   $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
   # Map surname claim
   $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
   # Map common name claim
   $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
   ```

   Questo mapping viene eseguito per fare in modo che gli attributi dell'archivio LDAP siano disponibili come attestazioni in AD FS per creare regole di controllo di accesso condizionale in AD FS. Consente inoltre AD FS di utilizzare schemi personalizzati negli archivi LDAP offrendo un modo semplice per eseguire il mapping degli attributi LDAP alle attestazioni.

3. Infine, è necessario registrare l'archivio LDAP con AD FS come trust del provider di attestazioni locale usando il cmdlet **Add-AdfsLocalClaimsProviderTrust** :

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

   Nell'esempio precedente si crea un trust del provider di attestazioni locale denominato "vendors". Si specificano le informazioni di connessione per AD FS per connettersi alla directory LDAP rappresentata dall'attendibilità del provider di `$vendorDirectory` attestazioni `-LdapServerConnection` locale assegnando al parametro. Si noti che nel passaggio uno è stata assegnata `$vendorDirectory` una stringa di connessione da usare per la connessione alla directory LDAP specifica. Infine, si specifica che gli attributi `$GivenName`LDAP `$Surname`, e `$CommonName` (di cui è stato eseguito il mapping alle attestazioni ad FS) devono essere usati per il controllo di accesso condizionale, inclusi i criteri di autenticazione a più fattori e le regole di autorizzazione di rilascio, nonché per il rilascio tramite attestazioni nei token di sicurezza emessi da ad FS. Per usare i protocolli attivi come WS-Trust con AD FS, è necessario specificare il parametro OrganizationalAccountSuffix, che consente AD FS di evitare ambiguità tra i trust del provider di attestazioni locali durante la manutenzione di una richiesta di autorizzazione attiva.

## <a name="see-also"></a>Vedi anche
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)

