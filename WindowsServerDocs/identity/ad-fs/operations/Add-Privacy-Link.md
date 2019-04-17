---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: Aggiungere un collegamento di Privacy
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 81a453b45693b8222bdfc0231885b506fdfcd2fc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="add-privacy-link"></a>Aggiungere un collegamento di Privacy 

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Per aggiungere il collegamento di privacy che viene visualizzato nella pagina di accesso, usare la sintassi e i cmdlet Windows PowerShell seguente.  

![Aggiungere un collegamento di privacy](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> Il `linkText`parametro in questo cmdlet non è obbligatorio solo se si utilizza un valore diverso da quello predefinito, cioè *Privacy*. Il vantaggio di utilizzare il valore predefinito è che le pagine sono localizzate in tutte le impostazioni locali del client. Dopo aver personalizzata la pagina di accesso, la personalizzazione ha la precedenza; Pertanto, è necessario applicarli a tutte le lingue che si desidera supportare. Tutto il contenuto personalizzato accetta un parametro di impostazioni locali. Quando si configura il contenuto localizzato, è necessario configurarlo con impostazioni locali senza country\ prima di tutto, ad esempio, "en", prima di configurare paese e impostazioni locali specifiche region\, ad esempio "en\-us".  

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[AD FS Sign-personalizzazione utente](AD-FS-user-sign-in-customization.md)  
