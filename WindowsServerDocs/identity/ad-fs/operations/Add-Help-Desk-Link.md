---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: Aggiungere un collegamento al supporto tecnico
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1673e6ee6357a9d59e8ac5891625d453bb434088
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358461"
---
# <a name="add-help-desk-link"></a>Aggiungere un collegamento al supporto tecnico 


## <a name="to-add-a-help-desk-link"></a>Per aggiungere un collegamento al supporto tecnico  
Per aggiungere il collegamento help desk visualizzato nella pagina\-di accesso, usare la sintassi e il cmdlet di Windows PowerShell seguenti.  

![aggiungere supporto tecnico](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> Il parametro `linkText` in questo cmdlet non è obbligatorio, a meno che venga usato un valore diverso da quello predefinito, cioè *Help*. L'uso del valore predefinito ha il vantaggio di essere localizzato in tutte le impostazioni locali del client. Dopo aver personalizzato la pagina, gli elementi personalizzati avranno la precedenza, quindi è necessario applicarli a tutte le lingue che si prevede di supportare.  


## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md)  
