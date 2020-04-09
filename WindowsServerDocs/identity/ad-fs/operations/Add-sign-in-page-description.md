---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Aggiungere l'accesso\-nella descrizione della pagina
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8d3cc69bde1c9126f97926802b53d049ed1ef501
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859994"
---
# <a name="add-sign-in-page-description"></a>Aggiungere l'accesso\-nella descrizione della pagina


## <a name="to-add-sign-in-page-description"></a>Per aggiungere accesso\-nella descrizione della pagina  
Per aggiungere un segno\-nella descrizione della pagina alla pagina Sign\-in, usare la sintassi e il cmdlet di Windows PowerShell seguenti.  

![aggiungere l'accesso nella descrizione](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> La stringa del parametro `SignInPageDescriptionText` supporta l'HTML puro con e senza tag. Pertanto, è possibile eseguire il cmdlet seguente senza usare il &lt;p&gt; tag.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

Dopo il segno di\-nella pagina è personalizzato, la personalizzazione ha la precedenza; pertanto, è necessario applicarli a tutte le lingue che si desidera supportare. Tutto il contenuto personalizzato accetta un parametro con le impostazioni locali. Quando si configura il contenuto localizzato, deve essere configurato con un paese\-meno locali prima, ad esempio, "en", prima di configurare paese e area\-specifico delle impostazioni locali, ad esempio "en\-us".  

## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md)  
