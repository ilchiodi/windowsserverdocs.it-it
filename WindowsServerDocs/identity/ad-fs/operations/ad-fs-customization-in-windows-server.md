---
ms.assetid: 25f5aff1-6abf-4dea-b531-f1d9943bc181
title: Personalizzazione di AD FS in Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b5aa22ad99529d99e2d7381a434916e8e749f185
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188757"
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>Personalizzazione di AD FS in Windows Server 2016


In risposta ai commenti e suggerimenti da organizzazioni usando AD FS, è stato aggiunto l'esperienza per le singole applicazioni protette da AD FS strumenti aggiuntivi per personalizzare l'utente di accesso.  
Oltre al contenuto per ogni applicazione web come testo della descrizione e collegamenti, ora è possibile specificare i temi intero web per ogni applicazione.  Ciò include logo, immagine, fogli di stile o un file OnLoad intero.  
  
## <a name="global-settings"></a>Impostazioni globali    
Per le impostazioni globali generale è possibile fare riferimento a [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx) fornito con AD FS in Windows Server 2012 R2.  
  
## <a name="pre-requisites"></a>Prerequisiti  
Sono necessari i prerequisiti seguenti prima di provare le procedure descritte in questo documento.  
  
-   AD FS in Windows Server 2016 TP4 o versione successiva  
  
## <a name="configure-ad-fs-relying-parties"></a>Configurare AD FS Relying party  
Per web Accedi relying party elementi e i temi possono essere configurati usando gli esempi di PowerShell riportato di seguito:  
  
### <a name="customize-messages"></a>Personalizzare i messaggi  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>Personalizzare l'immagine, logo e nome della società  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -Logo @{path="C:\Images\applogo.png"}  
    -Illustration @{path="C:\Images\appillustration.jpg"}  
```  
  
### <a name="customize-entire-page"></a>Personalizzare la pagina intera  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -OnLoadScriptPath @{path="c:\scripts\adfstheme\onload.js"}  
```  
  
## <a name="custom-themes-and-advanced-custom-themes"></a>Temi personalizzati e temi personalizzati avanzati  
  
Per i temi personalizzati, vedere [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx) e [Advanced Customization of AD FS Sign-in Pages.](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>Assegnazione di temi web personalizzati per ogni applicazione relying Party  
  
Per assegnare un tema personalizzato per ogni applicazione relying Party usare la procedura seguente:  
  
1. Creare un nuovo tema come una copia per impostazione predefinita, i temi globali in ADFS  
<code>New-AdfsWebTheme -Name AppSpecificTheme -SourceName default</code> 2.  Esportare il tema di personalizzazione <code>Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme</code>  
3. Personalizzare i file di tema (immagini, css, onLoad) - nell'editor preferito e sostituire il file di 4. Importare file personalizzati dal file system ad AD FS (avente come destinazione il nuovo tema) <code>Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}</code>  
5. Applicare il tema di nuovo, personalizzato per la relying Party specifico (o dell'applicazione relying Party) <code>Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme</code>  
  
## <a name="home-realm-discovery"></a>Individuazione dell'area di autenticazione principale  
Per l'individuazione dell'area di autenticazione Vedere personalizzazione [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="updated-password-page"></a>Pagina password aggiornata  
Per informazioni sulla personalizzazione della pagina password aggiornamento, vedere [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="customizing-and-alternate-ids"></a>ID alternativo e personalizzazione  
Gli utenti possono accedere a Active Directory Federation Services (ADFS)-applicazioni usando qualsiasi formato dell'ID utente che viene accettata da Active Directory Domain Services (AD DS) abilitate. Sono inclusi i nomi dell'entità utente (UPN) (johndoe@contoso.com) o di dominio completo di nomi di sam-account (contoso\johndoe o contoso.com\johndoe).  Per altre informazioni, vedere [ID di configurazione di accesso alternativo.](Configuring-Alternate-Login-ID.md)  
  
È inoltre possibile personalizzare la pagina di accesso di AD FS per consentire agli utenti finali alcuni hint sull'ID di accesso alternativo. È possibile farlo aggiungendo la descrizione personalizzata nella pagina di accesso per altre informazioni, vedere [Customizing the AD FS Sign-in Pages.](https://technet.microsoft.com/library/dn280950.aspx)   
  
Effettuare questa operazione personalizzando "Accesso con account aziendale" stringa precedente campo username.  Per informazioni, vedere [Advanced Customization of AD FS Sign-in Pages](https://technet.microsoft.com/library/dn636121.aspx).  

## <a name="additional-references"></a>Altri riferimenti 
[AD FS Sign-in personalizzazione dell'utente](AD-FS-user-sign-in-customization.md)  
