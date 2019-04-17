---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Aggiungere una descrizione della pagina di accesso
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 128a4ffd8d4b9dfcfe5f0e8e2df8a0e1505cbb24
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="add-sign-in-page-description"></a>Aggiungere una descrizione della pagina di accesso

>Si applica a: Windows Server 2016, Windows Server 2012 R2

## <a name="to-add-sign-in-page-description"></a>Per aggiungere una descrizione della pagina di accesso  
Per aggiungere una descrizione della pagina di accesso alla pagina di accesso, usare la sintassi e i cmdlet PowerShell di Windows PowerShell seguente.  

![Aggiungere i log nella descrizione](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> La stringa per il `SignInPageDescriptionText`parametro supporta l'HTML puro con i tag e senza. Pertanto, è possibile eseguire il cmdlet seguente senza usare il &lt;p&gt; tag.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

Dopo aver personalizzata la pagina di accesso, la personalizzazione ha la precedenza; Pertanto, è necessario applicarli a tutte le lingue che si desidera supportare. Tutto il contenuto personalizzato accetta un parametro di impostazioni locali. Quando si configura il contenuto localizzato, deve essere configurato con le impostazioni locali senza country\ prima di tutto, ad esempio, "en", prima di configurare paese e impostazioni locali specifiche region\, ad esempio "en\-us".  

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[AD FS Sign-personalizzazione utente](AD-FS-user-sign-in-customization.md)  
