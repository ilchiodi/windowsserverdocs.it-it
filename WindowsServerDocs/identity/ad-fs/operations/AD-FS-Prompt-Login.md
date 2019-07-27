---
title: Prompt AD FS = login
description: Domande frequenti per AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f4f284d4d970af8f8a672bd88be53f65ba70893f
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544640"
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Active Directory Federation Services prompt = supporto per i parametri di accesso

Nel documento seguente viene descritto il supporto nativo per il parametro prompt = login disponibile in AD FS.

## <a name="what-is-promptlogin"></a>Che cos'è prompt = login?

Quando le applicazioni devono richiedere una nuova autenticazione da Azure ad, vale a dire che devono Azure ad per autenticare di nuovo l'utente anche se l'utente è già stato autenticato, può inviare `prompt=login` il parametro ad Azure ad come parte dell'autenticazione. richiesta.

Quando questa richiesta è per un utente federato, Azure AD necessario informare il provider di identità, ad esempio AD FS, che la richiesta è per una nuova autenticazione.

Per impostazione predefinita, Azure ad viene convertito `prompt=login` in `wfresh=0` e `wauth=http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` quando si invia questo tipo di richieste di autenticazione all'IDP federato.

Questi parametri indicano:

- `wfresh=0`: esegue l'autenticazione aggiornata
- `wauth=http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`: Usa nome utente/password per la richiesta di autenticazione aggiornata

Ciò può causare problemi con scenari di autenticazione a più fattori e Intranet aziendali in cui si desidera un tipo di autenticazione diverso da nome utente e password `wauth` , come richiesto dal parametro.  

AD FS in Windows Server 2012 R2 con l'aggiornamento cumulativo di luglio 2016 è stato introdotto il `prompt=login` supporto nativo per il parametro. Ciò significa che ora Azure AD possibile inviare questo parametro così com'è per AD FS servizio come parte delle richieste di autenticazione Azure AD e Office 365.

## <a name="ad-fs-versions-that-support-promptlogin"></a>AD FS versioni che supportano prompt = login

Di seguito è riportato un elenco di ad FS versioni che supportano `prompt=login` il parametro.

- AD FS in Windows Server 2012 R2 con l'aggiornamento cumulativo del 2016 luglio
- AD FS in Windows Server 2016

## <a name="how-to-configure-a-federated-domain-to-send-promptlogin-to-ad-fs"></a>Come configurare un dominio federato per inviare la richiesta di accesso a AD FS

Usare il modulo Azure AD PowerShell per configurare l'impostazione.

> [!NOTE]
> La `prompt=login` funzionalità (abilitata `PromptLoginBehavior` dalla proprietà) è attualmente disponibile solo nella [versione 1,0 del modulo Azure ad PowerShell](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185), in cui i cmdlet hanno nomi che includono "MSOL", ad esempio set-MsolDomainFederationSettings.  Non è attualmente disponibile tramite "versione 2,0" del modulo Azure AD PowerShell, i cui cmdlet hanno nomi come "set-AzureAD\*".

1. Ottenere innanzitutto i valori correnti di `PreferredAuthenticationProtocol`, `SupportsMfa`e `PromptLoginBehavior` per il dominio federato eseguendo il comando PowerShell seguente:

```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | Format-List *
```

> [!NOTE]
> Per impostazione predefinita `Get-MsolDomainFederationSettings` , l'output di non visualizza determinate proprietà nella console. Per visualizzare tutte le proprietà, è necessario inviare`|`il relativo output a `Format-List *` per forzare l'output di tutte le proprietà dell'oggetto.

![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)

> [!NOTE]
> Se la `PreferredAuthenticationMethod` proprietà è vuota (`$null`) indica il comportamento predefinito di `TranslateToFreshPasswordAuth`.

2. Configurare il valore desiderato di `PromptLoginBehavior` eseguendo il comando seguente:

```powershell
    Set-MsolDomainFederationSettings –DomainName <your_domain_name> -PreferredAuthenticationProtocol <current_value_from_step1> -SupportsMfa <current_value_from_step1> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

Di seguito sono riportati i valori `PromptLoginBehavior` possibili del parametro e il relativo significato:

- **TranslateToFreshPasswordAuth**: indica il comportamento predefinito del Azure ad di conversione `prompt=login` in `wauth=http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` e `wfresh=0`.
- **NativeSupport**: indica che il `prompt=login` parametro verrà inviato così come è ad FS. Questo è il valore consigliato se AD FS si trova in Windows Server 2012 R2 con l'aggiornamento cumulativo di luglio 2016 o versione successiva.
- **Disabled**: indica che `wfresh=0` viene inviato solo a ad FS.
