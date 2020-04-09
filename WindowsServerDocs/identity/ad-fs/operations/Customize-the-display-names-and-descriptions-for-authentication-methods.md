---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: Personalizzare i nomi visualizzati e le descrizioni per i metodi di autenticazione
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 218643bbd5ada63b6bee2b91a7bace3f9959c0bd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816444"
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>Personalizzare i nomi visualizzati e le descrizioni per i metodi di autenticazione 


Per personalizzare i nomi visualizzati e le descrizioni dei metodi di autenticazione è possibile usare il cmdlet di PowerShell `Set-AdfsAuthenticationProviderWebContent` .  Per poter usare questo cmdlet, è prima di tutto necessario ottenere il nome del metodo di autenticazione che si vuole personalizzare.  A tale scopo, usare `Get-AdfsGlobalAuthenticationPolicy`.  Nell'esempio seguente si noterà che, nella pagina Sign\-in, viene visualizzato quanto segue: "Accedi con un certificato X. 509".  L'obiettivo è semplificare questa operazione per gli utenti.  
  
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
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md) 
