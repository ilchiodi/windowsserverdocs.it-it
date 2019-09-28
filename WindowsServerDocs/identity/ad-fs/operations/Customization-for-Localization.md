---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: Personalizzazione per la localizzazione
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ba3441d362960e054791ca3d6872dba6c7bd9a12
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357983"
---
# <a name="customization-for-localization"></a>Personalizzazione per la localizzazione 


È possibile localizzare il contenuto in lingue diverse dall'inglese. In caso di localizzazione, tenere presenti le seguenti considerazioni.  
  
Dopo aver personalizzato il contenuto, gli elementi personalizzati avranno la precedenza, quindi è necessario applicarli a tutte le lingue che si prevede di supportare. Tutto il contenuto personalizzato accetta un parametro con le impostazioni locali. Quando si configura il contenuto localizzato, configurarlo con un paese @ no__t-0less impostazioni locali prima, ad esempio, "en", prima di configurare un paese e una regione @ no__t-1specific impostazioni locali come "en @ no__t-2Inglese USA".  
  
Di seguito sono riportati altri esempi di codice.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
Di seguito sono riportati altri esempi di codice.  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
Se si desidera personalizzare il contenuto Web in lingue diverse dall'inglese che richiedono l'input di Unicode, è consigliabile utilizzare il Windows PowerShell ISE. Per altre informazioni, vedere [Introduzione a Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md) 
