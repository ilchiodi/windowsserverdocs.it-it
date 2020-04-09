---
ms.assetid: 1df78c2a-5054-4b54-8310-c48ea62e6e0b
title: Messaggi di errore personalizzati per la pagina di accesso AD ADFS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 31da3e65e69910817a78ab1007e897fb5a9ad683
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816434"
---
# <a name="custom-error-messages-for-ad-fs-sign-in-page"></a>Messaggi di errore personalizzati per la pagina di accesso AD ADFS  


Si possono configurare messaggi di errore personalizzati specifici per l'organizzazione. Nell'illustrazione seguente sono riportati la descrizione di una pagina di errore personalizzata e un messaggio di errore generico. Utilizzare i cmdlet di Windows PowerShell seguenti per personalizzare i messaggi di errore.  
  
![errore personalizzato](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom3.png)  
  
## <a name="customize-the-error-page-description"></a>Personalizzare la descrizione della pagina di errore  
Per personalizzare la descrizione della pagina di errore, utilizzare la sintassi e i cmdlet Windows PowerShell seguente.  
  

`Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" ` 

  
## <a name="customize-a-generic-error-message"></a>Personalizzare un messaggio di errore generico  
Per personalizzare il messaggio di errore generico, utilizzare la sintassi e i cmdlet Windows PowerShell seguente.  
  
 
`Set-AdfsGlobalWebContent -ErrorPageGenericErrorMessage "This is a generic error message.  Contact Contoso IT for assistance." ` 

  
## <a name="customize-an-authorization-error-message"></a>Personalizzare un messaggio di errore di autorizzazione  
Per personalizzare il messaggio di errore di autorizzazione, utilizzare la sintassi e i cmdlet Windows PowerShell seguente.  
  

    Set-AdfsGlobalWebContent -ErrorPageAuthorizationErrorMessage "You have received an Authorization error.  Contact Contoso IT for assistance."  

  
## <a name="customize-a-device-authentication-error-message"></a>Personalizzare un messaggio di errore di autenticazione del dispositivo  
Per personalizzare il messaggio di errore di autenticazione dispositivo, utilizzare la sintassi e i cmdlet Windows PowerShell seguente.  
  
 
`Set-AdfsGlobalWebContent -ErrorPageDeviceAuthenticationErrorMessage "Your device is not authorized.  Contact Contoso IT for assistance."`  
 
  
## <a name="customize-a-support-email-error-message"></a>Personalizzare un messaggio di errore di posta elettronica di supporto  
È possibile configurare un indirizzo di posta elettronica di supporto in ADFS. Se configurato, ADFS Visualizza automaticamente un collegamento agli utenti finali per i dettagli dell'errore di posta elettronica.  
  
Per personalizzare il messaggio di errore di posta elettronica di supporto, utilizzare la sintassi e i cmdlet Windows PowerShell seguente.  
  

    Set-AdfsGlobalWebContent -ErrorPageSupportEmail  "admin@contoso.com"  

  
## <a name="customize-a-relying-party-authorization-message"></a>Personalizzare un messaggio di autorizzazione della relying party  
È possibile configurare un messaggio di errore di relying party autorizzazione in ADFS.  
  
Per personalizzare il messaggio di errore relying party, usare la sintassi e i cmdlet Windows PowerShell seguente.  

    Set-AdfsRelyingPartyWebContent -Name fedpassive -ErrorPageAuthorizationErrorMessage "<p> You need to be a member of Security Auditors to access this site. Click <A href='http://accessrequest/'>here</A> for more information.</p>"  


## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md)    
