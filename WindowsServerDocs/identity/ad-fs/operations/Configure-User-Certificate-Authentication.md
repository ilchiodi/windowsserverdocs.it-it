---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: Configurare il supporto di ADFS per l'autenticazione del certificato utente
description: 
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.service: active-directory
ms.technology: identity-adfs
ms.openlocfilehash: 9941d4dd997e857874aceddc920ec7f9d8944a81
ms.sourcegitcommit: d351cdbb0bf2533d6db35626ebbc4924b3834309
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2018
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>Configurazione di ADFS per l'autenticazione del certificato utente

>Si applica a: Windows Server 2016, Windows Server 2012 R2

ADFS può essere configurato per x509 utilizzando una delle modalità di autenticazione del certificato utente descritto in [in questo articolo](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md). Questa funzionalità può essere usata [con Azure Active Directory](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/) o autonomamente, per abilitare i client e dispositivi il provisioning con i certificati utente per l'accesso AD FS risorse dalla intranet o extranet.

## <a name="prerequisites"></a>Prerequisiti
- Assicurarsi che i certificati utente sono considerati attendibili dai server tutti ADFS e WAP
- Assicurarsi che il certificato radice della catena di attendibilità per i certificati utente sia presente nell'archivio NTAuth in Active Directory
- Se con ADFS in modalità di autenticazione certificato alternativo, assicurarsi che i server ADFS e WAP dispongano dei certificati SSL che contengono il nome host AD FS preceduto da "certauth", ad esempio "certauth.fs.contoso.com" e che il traffico a questo nome host è consentito attraverso il firewall
- Se si usa l'autenticazione del certificato dalla rete extranet, assicurarsi che almeno un AIA e almeno un CDP o OCSP posizione dall'elenco specificato nei certificati sono accessibili da internet.
- Se si configura ADFS per l'autenticazione del certificato Azure AD, assicurarsi di aver configurato il [impostazioni di Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities) e [ADFS un'attestazione regole necessarie](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements) per certificato dell'autorità di certificazione e il numero di serie
- Anche per l'autenticazione del certificato, Azure AD per i client di Exchange ActiveSync, il certificato del client deve avere l'indirizzo di posta elettronica instradabile agli utenti di Exchange online nel nome dell'entità o il valore del nome RFC822 del campo nome alternativo oggetto. (Il valore RFC822 azure Active Directory corrisponde all'attributo indirizzo Proxy nella directory).

## <a name="configure-ad-fs-for-user-certificate-authentication"></a>Configurare ADFS per l'autenticazione del certificato utente  
- Abilitare l'autenticazione del certificato utente come intranet o un metodo di autenticazione della extranet di ADFS, tramite la console di gestione di ADFS o il cmdlet PowerShell Set-AdfsGlobalAuthenticationPolicy
- Verificare che l'intera catena di trust, tra cui i certificati intermedi, sia installato in ogni server ADFS e WAP. I certificati intermedi devono essere installati nell'archivio Autorità di certificazione intermedia computer locale nel server tutti ADFS e WAP.
- If you wish to use claims based on certificate fields and extensions in addition to EKU (claim type https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku), configure additional claim pass through rules on the Active Directory claims provider trust.  Per un elenco completo dei certificati disponibili attestazioni, vedere di seguito.  
- [Facoltativo] Configurare l'autorità di certificazione emittente consentiti per i certificati client secondo le indicazioni riportate nella sezione "Gestione delle autorità emittenti attendibili per l'autenticazione client" in [in questo articolo](https://technet.microsoft.com/en-us/library/dn786429(v=ws.11).aspx).

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>Configurare l'autenticazione del certificato trasparente per il browser Chrome nel desktop di Windows
Quando sono presenti nel computer che soddisfano i meccanismi di autenticazione client più certificati utente (ad esempio certificati Wi-Fi), il browser Chrome sul desktop di Windows verrà chiesto all'utente di selezionare il certificato appropriato. È possibile creare confusione per l'utente finale. Per ottimizzare l'esperienza, è possibile impostare un criterio per Chrome per la selezione automatica del certificato corretto per una migliore esperienza utente. Questo criterio può essere impostato manualmente apportando una modifica del Registro di sistema o configurato automaticamente tramite l'oggetto Criteri di gruppo (per impostare le chiavi del Registro di sistema). Ciò richiede i certificati client utente per l'autenticazione in ADFS di autorità emittenti distinti da altri casi di utilizzo. 

Per ulteriori informazioni su questa configurazione per Chrome, fare riferimento a questa [collegamento](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls).  


## <a name="troubleshooting"></a>Risoluzione dei problemi
- Se le richieste di autenticazione certificato non riesce con una risposta "Non contenuto da https://certauth.fs.contoso.com" 204 HTTP, verificare che la radice e certificati CA intermedi sono installati, rispettivamente, alla radice attendibile autorità di certificazione e autorità di certificazione intermedia archivio in tutti i server federativi certificati.
- Se le richieste di autenticazione certificato hanno esito negativo per motivi sconosciuti, esportare il certificato client in un file con estensione cer ed eseguire il comando 

`certutil -f -urlfetch -verify certificatefilename.cer`

Verificare eventuali CRL e delta resolve percorsi CRL.  Tieni presente che si trovano i percorsi delta CRL in base al contenuto del CRL di Base.

## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>Riferimento: Elenco completo dei certificati utente attestazione tipi e valori di esempio

|Tipo di attestazione|Valore di esempio
|-----|-----
|https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version | 3
|https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm | sha256RSA
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer | CN = CAOrg, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername | CN = CAOrg, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore | 12/05/2016 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter | 12/05/2017 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subject | E =user@contoso.com, CN = utenti, CN = Users, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname | E =user@contoso.com, CN = utenti, CN = Users, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata | {Dati certificato digitale con codifica Base64}
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | Bit DigitalSignature
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | KeyEncipherment
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier | 9D11941EC06FACCCCB1B116B56AA97F3987D620A
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier | ID chiave = d6 13 e3 6b bc e5 d8 15 52 0a fd 36 6a d5 0b 51 f3 0b 25 7f
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename | Utente
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/san | Altri nomi di entità: nome =user@contoso.com, nome RFC822 =user@contoso.com
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku | 1.3.6.1.4.1.311.10.3.4


