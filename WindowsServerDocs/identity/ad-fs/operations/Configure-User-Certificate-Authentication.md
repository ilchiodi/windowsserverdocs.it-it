---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: Configurare il supporto di AD FS per l'autenticazione del certificato utente
description: ''
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2d819ea036029fbe7cfde9ad5a445db6b2b42c96
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189705"
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>Configurazione di AD FS per l'autenticazione del certificato utente


ADFS può essere configurata per x509 usando una delle modalità di autenticazione del certificato utente descritto nella [questo articolo](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md). Questa funzionalità può essere usata [con Azure Active Directory](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/) o di per sé, per consentire ai client e dispositivi il provisioning con i certificati utente per l'accesso AD FS risorse dalla intranet o extranet.

## <a name="prerequisites"></a>Prerequisiti
- Assicurarsi che i certificati utente vengono considerati attendibili dai server tutti i AD FS e WAP
- Assicurarsi che il certificato radice della catena di trust per i certificati utente sia presente nell'archivio NTAuth in Active Directory
- Se si usa ADFS in modalità di autenticazione certificato alternativo, assicurarsi che i server AD FS e WAP abbiano i certificati SSL che contengono il nome host di AD FS il prefisso "certauth", ad esempio "com", e che il traffico a questo nome host è consentito attraverso il firewall
- Se si usa l'autenticazione del certificato dalla rete extranet, assicurarsi che almeno un AIA e almeno un CDP o OCSP posizione dall'elenco specificato in certificati sono accessibili da internet.
- Se si configura ADFS per l'autenticazione del certificato Azure AD, assicurarsi di aver configurato il [impostazioni di Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities) e il [regole necessarie attestazioni per AD FS](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements) per certificato dell'autorità emittente e il numero di serie
- Anche per l'autenticazione di certificato di Azure AD, per i client di Exchange ActiveSync, il certificato client deve avere l'indirizzo email instradabile degli utenti di Exchange online nel nome dell'entità o il valore del nome RFC822 del campo nome alternativo soggetto. (Azure Active Directory viene eseguito il mapping del valore RFC822 all'attributo di indirizzo Proxy nella directory.)

## <a name="configure-ad-fs-for-user-certificate-authentication"></a>Configurare AD FS per l'autenticazione del certificato utente  
- Abilitare l'autenticazione del certificato utente come intranet o un metodo di autenticazione della extranet di AD FS, usando la console di gestione di ADFS o il cmdlet di PowerShell Set-AdfsGlobalAuthenticationPolicy
- Assicurarsi che l'intera catena di trust, tra cui eventuali certificati intermedi, viene installato in ogni server AD FS e WAP. I certificati intermedi devono essere installati nell'archivio Autorità di certificazione intermedie computer locale nei server di tutto AD FS e WAP.
- Se si desidera usare le attestazioni in base a campi del certificato e le estensioni oltre all'utilizzo chiavi avanzato (tipo di attestazione https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku), configurare attestazioni aggiuntive pass-through regole nell'attendibilità del provider di attestazioni Active Directory.  Per un elenco completo delle attestazioni di certificato disponibili, vedere di seguito.  
- [Facoltativo] Configurare l'autorità di certificazione emittente consentiti per i certificati client usando le indicazioni fornite nella sezione "Gestione di autorità emittenti attendibili per l'autenticazione client" nel [questo articolo](https://technet.microsoft.com/library/dn786429(v=ws.11).aspx).

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>Configurare l'autenticazione del certificato senza problemi per il browser Chrome nei computer desktop Windows
Quando più certificati di utente (ad esempio, certificati Wi-Fi) sono presenti nel computer che soddisfano gli scopi di autenticazione client, il browser Chrome sul desktop di Windows richiederà all'utente di selezionare il certificato appropriato. Ciò potrebbe confondere l'utente finale. Per ottimizzare questa esperienza, è possibile impostare un criterio per Chrome per seleziona automaticamente il certificato appropriato per una migliore esperienza utente. Questo criterio può essere impostato manualmente, apportando una modifica del Registro di sistema o configurato automaticamente tramite oggetto Criteri di gruppo (per impostare le chiavi del Registro di sistema). Ciò richiede i certificati client utente per l'autenticazione con ADFS di disporre di autorità emittenti distinti da altri casi d'uso. 

Per altre informazioni su questa configurazione per Chrome, fare riferimento a questo [collegamento](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls).  


## <a name="troubleshooting"></a>Risoluzione dei problemi
- Se le richieste di autenticazione certificato ha esito negativo con un HTTP 204 "Nessun contenuto da https:\//certauth.fs.contoso.com" risposta, verificare che eventuali certificati CA intermedi e radice sono installati, rispettivamente, alla CA radice attendibile e certificato della CA intermedia vengono archiviati in tutti i server federativi.
- Se le richieste di autenticazione certificato non riescono per motivi sconosciuti, esportare il certificato client in un file con estensione cer ed eseguire il comando 

`certutil -f -urlfetch -verify certificatefilename.cer`

Assicurarsi che qualsiasi CRL e delta resolve percorsi CRL.  Si noti che i percorsi delta CRL sono stati trovati in base al contenuto del CRL di Base.

## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>Informazioni di riferimento: Elenco completo del certificato utente tipi di attestazione e i valori di esempio

|Tipo di attestazione|Valore di esempio
|-----|-----
|https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version | 3
|https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm | sha256RSA
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer | CN = CAOrg, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername | CN = CAOrg, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore | 12/05/2016 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter | 12/05/2017 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subject | E =user@contoso.com, CN = utente, CN = Users, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname | E =user@contoso.com, CN = utente, CN = Users, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata | {Certificato digitale dati con codifica Base64}
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | DigitalSignature
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | KeyEncipherment
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier | 9D11941EC06FACCCCB1B116B56AA97F3987D620A
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier | KeyID=d6 13 e3 6b bc e5 d8 15 52 0a fd 36 6a d5 0b 51 f3 0b 25 7f
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename | Utente
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/san | Altri nomi di entità: nome =user@contoso.com, nome RFC822 =user@contoso.com
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku | 1.3.6.1.4.1.311.10.3.4


