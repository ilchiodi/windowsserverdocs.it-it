---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: Personalizzazione per la localizzazione
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3cf209756c27c72836c7e2e1e58e84f3f5af2ca9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189182"
---
# <a name="customization-for-localization"></a>Personalizzazione per la localizzazione 


È possibile localizzare il contenuto in lingue diverse dall'inglese. In caso di localizzazione, tenere presenti le seguenti considerazioni.  
  
Dopo aver personalizzato il contenuto, gli elementi personalizzati avranno la precedenza, quindi è necessario applicarli a tutte le lingue che si prevede di supportare. Tutto il contenuto personalizzato accetta un parametro con le impostazioni locali. Quando si configura il contenuto localizzato, configurarlo con un paese\-meno locali prima, ad esempio, "en", prima di configurare un paese e area geografica\-specifiche delle impostazioni locali, ad esempio "en\-us".  
  
Di seguito sono riportati altri esempi di codice.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
Di seguito sono riportati altri esempi di codice.  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
Se si desidera personalizzare il contenuto web in lingue diverse dall'inglese che richiedono l'input in Unicode, è consigliabile usare Windows PowerShell ISE. Per altre informazioni, vedere [Introduzione a Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Altri riferimenti 
[AD FS Sign-in personalizzazione dell'utente](AD-FS-user-sign-in-customization.md) 
