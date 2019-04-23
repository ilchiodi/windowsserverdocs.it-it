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
ms.openlocfilehash: 699622a8a075dd6c78ab1b536dce2abfee642e9e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855192"
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>Personalizzare i nomi visualizzati e le descrizioni per i metodi di autenticazione 

>Si applica a: Windows Server 2016, Windows Server 2012 R2

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
