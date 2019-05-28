---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: Personalizzare i nomi visualizzati e le descrizioni per i metodi di autenticazione
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b9702873d42e0a72e510ac022d8d7fb04b45dab9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189174"
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>Personalizzare i nomi visualizzati e le descrizioni per i metodi di autenticazione 


Per personalizzare i nomi visualizzati e le descrizioni dei metodi di autenticazione è possibile usare il cmdlet di PowerShell `Set-AdfsAuthenticationProviderWebContent` .  Per poter usare questo cmdlet, è prima di tutto necessario ottenere il nome del metodo di autenticazione che si vuole personalizzare.  A tale scopo, usare `Get-AdfsGlobalAuthenticationPolicy`.  Nell'esempio riportato di seguito vediamo che, al nostro accesso\-nella pagina, viene visualizzato il seguente:  "Accedi con un certificato X.509".  L'obiettivo è semplificare questa operazione per gli utenti.  
  
![personalizzare displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)  
  
A tale scopo, verrà modificato il testo visualizzato del metodo di autenticazione.  
  
 
    Get-AdfsGlobalAuthenticationPolicy  
      
    AdditionalAuthenticationProvider  : {}  
    DeviceAuthenticationEnabled   : False  
    PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    WindowsIntegratedFallbackEnabled  : True  
      
    Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"  
  
  
![personalizzare displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)  
  
Come si può vedere, il messaggio visualizzato è cambiato.  
  
![personalizzare displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)  

## <a name="additional-references"></a>Altri riferimenti 
[AD FS Sign-in personalizzazione dell'utente](AD-FS-user-sign-in-customization.md) 
