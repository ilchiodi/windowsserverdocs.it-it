---
ms.assetid: 25f5aff1-6abf-4dea-b531-f1d9943bc181
title: Personalizzazione di AD FS in Windows Server 2016
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e4d463f5f25fe85dc95d767c9c75b722e60b012
ms.sourcegitcommit: a2699e93a0a19cb138c1fde0c9af36774a12f865
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2017
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>Personalizzazione di AD FS in Windows Server 2016

>Si applica a: Windows Server 2016

In risposta al feedback delle organizzazioni utilizza ADFS, abbiamo aggiunto altri strumenti per personalizzare l'utente l'accesso per singole applicazioni protetti da ADFS.  
Oltre a specificare il contenuto per ogni applicazione web, ad esempio testo della descrizione e i collegamenti, ora è possibile specificare i temi web intero per ogni applicazione.  Sono inclusi logo, immagine, fogli di stile o un file intero onload.js.  
  
## <a name="global-settings"></a>Impostazioni globali    
Le impostazioni globali generale è possibile fare riferimento a [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx) che fornite con AD FS in Windows Server 2012 R2.  
  
## <a name="pre-requisites"></a>Prerequisiti  
I seguenti prerequisiti sono necessari prima di eseguire le procedure descritte in questo documento.  
  
-   AD FS in Windows Server 2016 TP4 o versione successiva  
  
## <a name="configure-ad-fs-relying-parties"></a>Configurare AD FS Relying party  
Per ogni web accesso relying party temi e gli elementi possono essere configurati con gli esempi di PowerShell seguenti:  
  
### <a name="customize-messages"></a>Personalizzare i messaggi  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>Personalizzare l'immagine del logo e nome della società  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -Logo @{path="C:\Images\applogo.png"}  
    -Illustration @{path="C:\Images\appillustration.jpg"}  
```  
  
### <a name="customize-entire-page"></a>Personalizzare l'intera pagina  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -OnLoadScriptPath @{path="c:\scripts\adfstheme\onload.js"}  
```  
  
## <a name="custom-themes-and-advanced-custom-themes"></a>Temi personalizzati e avanzati temi personalizzati  
  
Per consultare temi personalizzati [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx) e [Advanced Customization of AD FS Sign-in Pages.](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>L'assegnazione di temi web personalizzati per ogni applicazione relying Party  
  
Per assegnare un tema personalizzato per ogni applicazione relying Party la procedura seguente:  
  
1. Creare un nuovo tema come copia per impostazione predefinita, il tema globale in ADFS  
<code>New-AdfsWebTheme -Name AppSpecificTheme -SourceName default</code>  
2.  Esportare il tema per la personalizzazione <code>Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme</code>3.personalizzare i file del tema (immagini, css, onload.js) - nell'editor preferito o sostituire il file 4. Importare i file personalizzati dal file system per AD FS (il nuovo tema di destinazione) <code>Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}</code>5.applicare il tema personalizzato, di nuovo a specifici RP (o dell'applicazione relying Party)
<code>Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme</code>  
  
## <a name="home-realm-discovery"></a>Individuazione dell'area di autenticazione  
Per l'individuazione dell'area di autenticazione Vedere personalizzazione [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="updated-password-page"></a>Pagina password aggiornata  
Per informazioni su come personalizzare la pagina password aggiornamento vedere [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="customizing-and-alternate-ids"></a>ID di personalizzazione e alternativo  
Gli utenti possono accedere per Active Directory Federation Services (ADFS)-abilitata di applicazioni mediante alcuna forma di identificatore utente accettato dai servizi di dominio Active Directory (AD DS). Sono inclusi i nomi dell'entità utente (UPN) (johndoe@contoso.com) o di dominio completo di nomi di account sam (contoso\johndoe o contoso.com\johndoe #).  Per ulteriori informazioni, vedere questo [ID di configurazione di accesso alternativo.](Configuring-Alternate-Login-ID.md)  
  
Si consiglia inoltre di personalizzare la pagina di accesso di ADFS per fornire agli utenti finali alcuni suggerimento per l'ID di accesso alternativo. È possibile farlo aggiungendo la descrizione della pagina di accesso personalizzata per ulteriori informazioni, vedere [personalizzazione delle pagine AD FS Sign-in.](https://technet.microsoft.com/library/dn280950.aspx)   
  
È anche possibile farlo personalizzando la stringa "accedere con account aziendale" sopra il campo nome utente.  Per informazioni su questo vedere [Advanced Customization of AD FS Sign-in Pages](https://technet.microsoft.com/library/dn636121.aspx).  

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[AD FS Sign-personalizzazione utente](AD-FS-user-sign-in-customization.md)  
