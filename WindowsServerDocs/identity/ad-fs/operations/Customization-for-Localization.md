---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: Personalizzazione per la localizzazione
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: eab7062c6678c0e9f3ef970ef9cff97fa63dd868
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816394"
---
# <a name="customization-for-localization"></a>Personalizzazione per la localizzazione 


È possibile localizzare il contenuto in lingue diverse dall'inglese. In caso di localizzazione, tenere presenti le seguenti considerazioni.  
  
Dopo aver personalizzato il contenuto, gli elementi personalizzati avranno la precedenza, quindi è necessario applicarli a tutte le lingue che si prevede di supportare. Tutto il contenuto personalizzato accetta un parametro con le impostazioni locali. Quando si configura il contenuto localizzato, è necessario configurarlo con un paese\-meno impostazioni locali, ad esempio, "en", prima di configurare un paese e una regione\-impostazioni locali specifiche, ad esempio "en\-US".  
  
Di seguito sono riportati altri esempi di codice.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
Di seguito sono riportati altri esempi di codice.  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
Se si desidera personalizzare il contenuto Web in lingue diverse dall'inglese che richiedono l'input di Unicode, è consigliabile utilizzare il Windows PowerShell ISE. Per altre informazioni, vedere [Introduzione a Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md) 
