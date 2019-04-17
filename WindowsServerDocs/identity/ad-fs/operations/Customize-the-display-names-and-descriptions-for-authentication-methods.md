---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: Personalizzare i nomi visualizzati e le descrizioni dei metodi di autenticazione
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 699622a8a075dd6c78ab1b536dce2abfee642e9e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>Personalizzare i nomi visualizzati e le descrizioni dei metodi di autenticazione 

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Per personalizzare i nomi visualizzati e le descrizioni dei metodi di autenticazione è possibile utilizzare il `Set-AdfsAuthenticationProviderWebContent`cmdlet di PowerShell.  Per usare questo cmdlet, è innanzitutto necessario ottenere il nome del metodo di autenticazione che si desidera personalizzare.  Questa operazione può essere eseguita tramite `Get-AdfsGlobalAuthenticationPolicy`.  Nell'esempio riportato di seguito vediamo che, nel nostro della pagina di accesso, viene visualizzato il seguente: "accedere con un certificato x. 509".  Vogliamo semplificare questa operazione per gli utenti.  
  
![Personalizzare displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)  
  
A tale scopo, otteniamo il nome del metodo di autenticazione e verrà modificato il testo visualizzato.  
  
 
    Get-AdfsGlobalAuthenticationPolicy  
      
    AdditionalAuthenticationProvider  : {}  
    DeviceAuthenticationEnabled   : False  
    PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    WindowsIntegratedFallbackEnabled  : True  
      
    Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"  
  
  
![Personalizzare displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)  
  
Vediamo ora che il messaggio visualizzato è cambiato.  
  
![Personalizzare displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)  

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[AD FS Sign-personalizzazione utente](AD-FS-user-sign-in-customization.md) 
