---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: Aggiungi collegamento alla home page
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb903c62e717e36099934e64e1c939a502f691a3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="add-home-link"></a>Aggiungi collegamento alla home page 

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Per aggiungere il collegamento alla home page visualizzato nella pagina di accesso, usare la sintassi e i cmdlet Windows PowerShell seguente. 


![Aggiungi collegamento alla home page](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> Il `linkText`parametro in questo cmdlet non è obbligatorio solo se si utilizza un valore diverso da quello predefinito, cioè *Home*. Il vantaggio di utilizzare il valore predefinito è che vengono localizzate per tutte le impostazioni locali del client. Dopo aver personalizzata la pagina di accesso, la personalizzazione ha la precedenza; Pertanto, è necessario applicarli a tutte le lingue che si desidera supportare.

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[AD FS Sign-personalizzazione utente](AD-FS-user-sign-in-customization.md)  
