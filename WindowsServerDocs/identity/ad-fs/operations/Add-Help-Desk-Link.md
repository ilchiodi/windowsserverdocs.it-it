---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: Aggiungere un collegamento al supporto tecnico
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb186c3ba5cfb3acb9bfd0c3139b09b992fb8863
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190215"
---
# <a name="add-help-desk-link"></a>Aggiungere un collegamento al supporto tecnico 


## <a name="to-add-a-help-desk-link"></a>Per aggiungere un collegamento al supporto tecnico  
Per aggiungere il collegamento al supporto tecnico che viene visualizzato il segno\-nella pagina, utilizzare la sintassi e i cmdlet di Windows PowerShell seguente.  

![aggiungere supporto tecnico](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> Il parametro `linkText` in questo cmdlet non è obbligatorio, a meno che venga usato un valore diverso da quello predefinito, cioè *Help*. L'uso del valore predefinito ha il vantaggio di essere localizzato in tutte le impostazioni locali del client. Dopo aver personalizzato la pagina, gli elementi personalizzati avranno la precedenza, quindi è necessario applicarli a tutte le lingue che si prevede di supportare.  


## <a name="additional-references"></a>Altri riferimenti 
[AD FS Sign-in personalizzazione dell'utente](AD-FS-user-sign-in-customization.md)  
