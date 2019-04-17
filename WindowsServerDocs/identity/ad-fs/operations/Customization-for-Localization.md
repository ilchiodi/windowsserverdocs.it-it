---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: Personalizzazione per la localizzazione
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ac206d5aa8af970b65a014955ac66a8cf2835eb6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="customization-for-localization"></a>Personalizzazione per la localizzazione 

>Si applica a: Windows Server 2016, Windows Server 2012 R2

È possibile localizzare il contenuto in lingue diverse dall'inglese. In caso di localizzazione, tenere presente quanto segue.  
  
Dopo aver personalizzato il contenuto, la personalizzazione ha la precedenza; Pertanto, è necessario applicarli a tutte le lingue che si desidera supportare. Tutto il contenuto personalizzato accetta un parametro di impostazioni locali. Quando si configura il contenuto localizzato, configurarlo con le impostazioni locali senza country\ prima di tutto, ad esempio, "en", prima di configurare paese e impostazioni locali specifiche region\, ad esempio "en\-us".  
  
Di seguito vengono riportati alcuni esempi di codice aggiuntivo.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
Di seguito vengono riportati alcuni esempi di codice aggiuntivo.  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
Se si desidera personalizzare il contenuto web in lingue diverse dall'inglese che richiedono l'input in Unicode, è consigliabile utilizzare Windows PowerShell ISE. Per ulteriori informazioni vedere [Introduzione a Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[AD FS Sign-personalizzazione utente](AD-FS-user-sign-in-customization.md) 
