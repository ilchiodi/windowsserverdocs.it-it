---
title: Richiesta di AD FS = account di accesso
description: Domande frequenti per AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8b98cdc27c9bd01d9855e98965f59ebe5257ed5c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Active Directory Federation Services prompt = supporto parametro di accesso
In questo documento descrive il supporto nativo per il parametro di accesso = prompt dei comandi disponibili in ADFS.

## <a name="what-is-promptlogin"></a>Che cos'è il prompt dei comandi = accesso?  

Alcune applicazioni di Office 365 (con l'autenticazione moderna abilitata) inviano il parametro di accesso = prompt dei comandi ad Azure AD come parte di ogni richiesta di autenticazione.  By default, Azure AD translates this into two parameters: <code><b>wauth</b>=urn:oasis:names:tc:SAML:1.0:am:password</code>, and <code><b>wfresh</b>=0</code>.

Questo può causare problemi di rete intranet aziendale e scenari di multi-factor authentication in cui si desidera un tipo di autenticazione diverso dal nome utente e password.  

ADFS in Windows Server 2012 R2 con aggiornamento cumulativo di luglio 2016 introdotto il supporto nativo per il parametro di accesso = prompt dei comandi.  Ciò significa, hai ora la possibilità di configurazione di Azure AD per questo parametro come inviare-è il servizio ADFS come parte di Azure Active Directory e le richieste di autenticazione di Office 365.

### <a name="ad-fs-versions-that-support-promptlogin"></a>Le versioni di AD FS che supportano la richiesta di accesso =
Di seguito è riportato un elenco delle versioni di ADFS che supportano il parametro di accesso = prompt dei comandi.

- Aggiornamento cumulativo di ADFS in Windows Server 2012 R2 con luglio 2016

- ADFS in Windows Server 2016

## <a name="how-do-to-configure-your-azure-ad-tenant-to-send-promptlogin-to-ad-fs"></a>Come fare per configurare tenant di Azure AD per inviare una richiesta = accesso ad ADFS

Usare il modulo di Azure AD PowerShell per configurare l'impostazione.

> [!NOTE]
> The prompt=login capability (enabled by the PromptLoginBehavior property) is currently available only in the [‘version 1.0’ Azure AD Powershell module](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185), in which the cmdlets have names that include “Msol”, such as Set-MsolDomainFederationSettings.  Non è attualmente disponibile tramite ' versione 2.0' Azure AD PowerShell modulo, i cmdlet di cui hanno nomi come "Set-AzureAD\ *".

Per configurare richiesta = comportamento di accesso, la sintassi del cmdlet seguente:

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

 
 Sono disponibili i valori delle proprietà PreferredAuthenticationProtocol SupportsMfa e PromptLoginBehavior visualizzando l'output del cmdlet: ![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)
```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | fl *
 ```
> [!NOTE]
> Per impostazione predefinita, quando si esegue Get-MsolDomainFederationSettings, alcune proprietà non vengono visualizzati nella console.  Per visualizzare questi parametri è consigliabile utilizzare il | FL * per forzare l'output di tutte le proprietà dell'oggetto.


Di seguito è informazioni il parametro PromptLoginBehavior e le relative impostazioni.
   
   - <b>TranslateToFreshPasswordAuth</b> significa che il comportamento di Azure AD predefinito di invio <b>wauth</b> e <b>wfresh</b> ad ADFS invece richiesta = account di accesso
   - <b>NativeSupport</b> significa che il parametro di accesso = prompt dei comandi verrà inviato come è per AD FS
   - <b>Disabilitato</b> significa che non verrà inviato ad ADFS

