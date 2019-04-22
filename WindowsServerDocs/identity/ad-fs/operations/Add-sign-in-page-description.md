---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Aggiungere l'accesso\-nella descrizione della pagina
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 553cdcf2ac98b23d06cc43a64220cf8433117fc8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814012"
---
# <a name="add-sign-in-page-description"></a>Aggiungere l'accesso\-nella descrizione della pagina

>Si applica a: Windows Server 2016, Windows Server 2012 R2

## <a name="to-add-sign-in-page-description"></a>Per aggiungere accesso\-nella descrizione della pagina  
Per aggiungere un segno\-nella descrizione della pagina per il segno\-nella pagina, utilizzare la sintassi e i cmdlet di Windows PowerShell seguente.  

![aggiungere l'accesso nella descrizione](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> La stringa per il `SignInPageDescriptionText` supporta l'HTML puro con i tag e senza parametri. Pertanto, è possibile eseguire il cmdlet seguente senza usare il &lt;p&gt; tag.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

Dopo il segno di\-nella pagina è personalizzato, la personalizzazione ha la precedenza; pertanto, è necessario applicarli a tutte le lingue che si desidera supportare. Tutto il contenuto personalizzato accetta un parametro con le impostazioni locali. Quando si configura il contenuto localizzato, deve essere configurato con un paese\-meno locali prima, ad esempio, "en", prima di configurare paese e area\-specifico delle impostazioni locali, ad esempio "en\-us".  

## <a name="additional-references"></a>Altri riferimenti 
[AD FS Sign-in personalizzazione dell'utente](AD-FS-user-sign-in-customization.md)  
