---
title: Prompt dei comandi di AD FS = account di accesso
description: Domande frequenti per AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6a4b6cfe98064181824e210be9031a0f67cb4b75
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824632"
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Active Directory Federation Services prompt = login parametro supporto
Il documento seguente viene descritto il supporto nativo per il parametro prompt = account di accesso che è disponibile in AD FS.

## <a name="what-is-promptlogin"></a>What ' s prompt = login?  

Alcune applicazioni di Office 365 (con autenticazione moderna abilitata) inviano il parametro = messaggio di richiesta di accesso ad Azure AD come parte di ogni richiesta di autenticazione.  Per impostazione predefinita, Azure AD lo traduce in due parametri: <code> <b> wauth </b> =urn:oasis:names:tc:SAML:1.0:am:password </code>, e <code> <b> wfresh </b> =0 </code> .

Ciò può causare problemi relativi alla rete intranet aziendale e gli scenari multi-factor authentication in cui si desidera ottenere un tipo di autenticazione diverso dal nome utente e password.  

ADFS in Windows Server 2012 R2 con aggiornamento cumulativo di luglio 2016 ha introdotto il supporto nativo per il parametro di accesso = prompt.  Ciò significa, è necessario ora l'opzione di configurazione di Azure AD per l'invio di questo parametro come-è il servizio AD FS come parte di Azure Active Directory e le richieste di autenticazione di Office 365.

### <a name="ad-fs-versions-that-support-promptlogin"></a>Versioni di AD FS che supportano prompt = login
Di seguito è riportato un elenco delle versioni di AD FS che supportano il parametro di accesso = prompt.

- ADFS in Windows Server 2012 R2 con di luglio 2016 aggiornamento cumulativo

- ADFS in Windows Server 2016

## <a name="how-do-to-configure-your-azure-ad-tenant-to-send-promptlogin-to-ad-fs"></a>Procedura per configurare il tenant di Azure AD per l'invio dei messaggi di richiesta = account di accesso ad AD FS

Usare il modulo Azure AD PowerShell per configurare l'impostazione.

> [!NOTE]
> La funzionalità di accesso = messaggio di richiesta (abilitata per la proprietà PromptLoginBehavior) è attualmente disponibile solo nel [' modulo di Azure AD Powershell versione 1.0'](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185), in cui i cmdlet hanno nomi che includono "Msol", ad esempio Set-MsolDomainFederationSettings.  Non è attualmente disponibile tramite ' versione 2.0' modulo Azure AD PowerShell, cmdlet di cui hanno nomi come "Set-AzureAD\*".

Per configurare i prompt = comportamento di account di accesso, la sintassi del cmdlet riportato di seguito:

Esempio 1:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PreferredAuthenticationProtocol <your current protocol setting> 
```

Esempio 2:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -SupportsMfa <$True|$False>
```

Esempio 3:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

 
 I valori delle proprietà PreferredAuthenticationProtocol SupportsMfa e PromptLoginBehavior sono reperibile visualizzando l'output del cmdlet: ![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)
```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | fl *
 ```
> [!NOTE]
> Per impostazione predefinita, quando si esegue Get-MsolDomainFederationSettings, alcune proprietà non vengono visualizzate nella console.  Per visualizzare questi parametri, è consigliabile usare il | FL * per forzare l'output di tutte le proprietà dell'oggetto.


Di seguito è riportate altre informazioni sul parametro PromptLoginBehavior e le relative impostazioni.
   
   - <b>TranslateToFreshPasswordAuth</b> significa che il comportamento di Azure AD predefinito di invio <b>wauth</b> e <b>wfresh</b> ad AD FS anziché prompt = login
   - <b>NativeSupport</b> significa che verrà inviato il parametro di accesso = messaggio di richiesta è per AD FS
   - <b>Disabilitato</b> significa che non verrà inviato al servizio AD FS

