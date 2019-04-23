---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: Aggiungere un collegamento all'informativa sulla privacy
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 81a453b45693b8222bdfc0231885b506fdfcd2fc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836802"
---
# <a name="add-privacy-link"></a>Aggiungere un collegamento all'informativa sulla privacy 

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Per aggiungere il collegamento di privacy che viene visualizzato il segno\-nella pagina, utilizzare la sintassi e i cmdlet Windows PowerShell seguente.  

![aggiungere un collegamento di privacy](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> Il parametro `linkText` in questo cmdlet non è obbligatorio, a meno che venga usato un valore diverso da quello predefinito, cioè *Privacy*. L'uso del valore predefinito presenta il vantaggio che le pagine sono localizzate in tutte le impostazioni locali del client. Dopo il segno di\-nella pagina è personalizzato, la personalizzazione ha la precedenza; pertanto, è necessario applicarli a tutte le lingue che si desidera supportare. Tutto il contenuto personalizzato accetta un parametro con le impostazioni locali. Quando si configura il contenuto localizzato, è necessario configurarlo con un paese\-meno locali prima, ad esempio, "en", prima di configurare paese e area\-specifico delle impostazioni locali, ad esempio "en\-us".  

## <a name="additional-references"></a>Altri riferimenti 
[AD FS Sign-in personalizzazione dell'utente](AD-FS-user-sign-in-customization.md)  
