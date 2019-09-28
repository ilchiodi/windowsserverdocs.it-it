---
ms.assetid: 25f5aff1-6abf-4dea-b531-f1d9943bc181
title: Personalizzazione di AD FS in Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4b0ea70bd9346bf8abee4e0d96a8915e29cac462
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357776"
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>Personalizzazione di AD FS in Windows Server 2016


In risposta ai commenti e suggerimenti delle organizzazioni che usano AD FS, sono stati aggiunti altri strumenti per personalizzare l'esperienza di accesso degli utenti per le singole applicazioni protette da AD FS.  
Oltre a specificare contenuto Web per applicazione come testo descrittivo e collegamenti, è ora possibile specificare interi temi Web per applicazione.  Sono inclusi logo, illustrazione, fogli di stile o un intero file OnLoad. js.  
  
## <a name="global-settings"></a>Impostazioni globali    
Per le impostazioni globali generali, è possibile fare riferimento alla [personalizzazione delle pagine di accesso ad FS](https://technet.microsoft.com/library/dn280950.aspx) fornite con ad FS in Windows Server 2012 R2.  
  
## <a name="pre-requisites"></a>Prerequisiti  
Prima di eseguire le procedure descritte in questo documento, è necessario disporre dei prerequisiti seguenti.  
  
-   AD FS in Windows Server 2016 TP4 o versione successiva  
  
## <a name="configure-ad-fs-relying-parties"></a>Configurare AD FS relying party  
Gli elementi e i temi Web di accesso per relying party possono essere configurati usando gli esempi di PowerShell seguenti:  
  
### <a name="customize-messages"></a>Personalizzare i messaggi  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>Personalizzare il nome della società, il logo e l'immagine  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -Logo @{path="C:\Images\applogo.png"}  
    -Illustration @{path="C:\Images\appillustration.jpg"}  
```  
  
### <a name="customize-entire-page"></a>Personalizza intera pagina  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -OnLoadScriptPath @{path="c:\scripts\adfstheme\onload.js"}  
```  
  
## <a name="custom-themes-and-advanced-custom-themes"></a>Temi personalizzati e temi personalizzati avanzati  
  
Per i temi personalizzati, vedere [personalizzazione delle pagine di accesso ad FS](https://technet.microsoft.com/library/dn280950.aspx) e [personalizzazione avanzata di ad FS pagine di accesso.](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>Assegnazione di temi Web personalizzati per RP  
  
Per assegnare un tema personalizzato per RP, attenersi alla procedura seguente:  
  
1. Creare un nuovo tema come copia del tema globale predefinito in AD FS  
`New-AdfsWebTheme -Name AppSpecificTheme -SourceName default`  
2. Esporta il tema per la personalizzazione  
`Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme`  
3. Personalizzare i file dei temi (images, CSS, OnLoad. js) nell'editor preferito o sostituire il file  
4. Importa i file personalizzati dal file system al AD FS (destinati al nuovo tema)  
`Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}`  
5. Applicare il nuovo tema personalizzato alla RP specifica (o RP)  
`Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme`  
  
## <a name="home-realm-discovery"></a>Individuazione dell'area di autenticazione principale  
Per la personalizzazione dell'area di autenticazione principale, vedere [personalizzazione delle pagine di accesso ad FS](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="updated-password-page"></a>Pagina password aggiornata  
Per informazioni sulla personalizzazione della pagina di aggiornamento della password, vedere [personalizzazione delle pagine di accesso ad FS](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="customizing-and-alternate-ids"></a>Personalizzazione e ID alternativi  
Gli utenti possono accedere alle applicazioni abilitate per Active Directory Federation Services (AD FS) utilizzando qualsiasi forma di identificatore utente accettato da Active Directory Domain Services (AD DS). Sono inclusi i nomi dell'entità utente (UPN) (johndoe@contoso.com) o i nomi di account SAM qualificati del dominio (CONTOSO\johndoe o contoso. com\johndoe).  Per ulteriori informazioni, vedere Configurazione di un [ID di accesso alternativo.](Configuring-Alternate-Login-ID.md)  
  
Potrebbe inoltre essere necessario personalizzare la pagina di accesso AD FS per fornire agli utenti finali un suggerimento sull'ID di accesso alternativo. È possibile eseguire questa operazione aggiungendo la descrizione della pagina di accesso personalizzata per ulteriori informazioni, vedere [personalizzazione delle pagine di accesso ad FS.](https://technet.microsoft.com/library/dn280950.aspx)   
  
È anche possibile eseguire questa operazione personalizzando la stringa "Accedi con account aziendale" sopra il campo nome utente.  Per informazioni su questa pagina, vedere [personalizzazione avanzata delle pagine di accesso ad FS](https://technet.microsoft.com/library/dn636121.aspx).  

## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md)  
