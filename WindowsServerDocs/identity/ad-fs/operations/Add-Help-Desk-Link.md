---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: Aggiungi collegamento al supporto tecnico
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d16cc0a75bfe636c29b44687b669e87f31b69ce
ms.sourcegitcommit: 76e57a5453d6ee9a04dcff6a8cca087132cb1d5f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/20/2018
---
# <a name="add-help-desk-link"></a>Aggiungi collegamento al supporto tecnico 

>Si applica a: Windows Server 2016, Windows Server 2012 R2


## <a name="to-add-a-help-desk-link"></a>Per aggiungere un collegamento al supporto tecnico  
Per aggiungere il collegamento al supporto tecnico che viene visualizzato nella pagina di accesso, usare la sintassi e i cmdlet PowerShell di Windows PowerShell seguente.  

![Aggiungere supporto tecnico](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> Il `linkText`parametro in questo cmdlet non è obbligatorio solo se si utilizza un valore diverso da quello predefinito, cioè *Guida*. Il vantaggio di utilizzare il valore predefinito è che vengono localizzate per tutte le impostazioni locali del client. Dopo aver personalizzata la pagina, la personalizzazione ha la precedenza; Pertanto, è necessario applicarli a tutte le lingue che si desidera supportare.  


## <a name="additional-references"></a>Riferimenti aggiuntivi 
[AD FS Sign-personalizzazione utente](AD-FS-user-sign-in-customization.md)  
