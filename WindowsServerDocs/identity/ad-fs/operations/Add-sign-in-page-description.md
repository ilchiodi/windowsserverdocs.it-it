---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Aggiungere l'accesso\-nella descrizione della pagina
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3b34a4e54aebd5b9dc3655eecd770a25f7ea97cf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407741"
---
# <a name="add-sign-in-page-description"></a>Aggiungere l'accesso\-nella descrizione della pagina


## <a name="to-add-sign-in-page-description"></a>Per aggiungere accesso\-nella descrizione della pagina  
Per aggiungere un segno\-nella descrizione della pagina alla pagina Sign\-in, usare la sintassi e il cmdlet di Windows PowerShell seguenti.  

![aggiungere l'accesso nella descrizione](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> La stringa per il parametro `SignInPageDescriptionText` supporta sia HTML puro con tag che senza. Pertanto, è possibile eseguire il cmdlet seguente senza usare il &lt;p&gt; tag.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

Dopo il segno di\-nella pagina è personalizzato, la personalizzazione ha la precedenza; pertanto, è necessario applicarli a tutte le lingue che si desidera supportare. Tutto il contenuto personalizzato accetta un parametro con le impostazioni locali. Quando si configura il contenuto localizzato, deve essere configurato con un paese\-meno locali prima, ad esempio, "en", prima di configurare paese e area\-specifico delle impostazioni locali, ad esempio "en\-us".  

## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md)  
