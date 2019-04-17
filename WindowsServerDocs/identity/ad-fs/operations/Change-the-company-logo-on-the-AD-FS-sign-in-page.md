---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: "Modifica il logo della società nella pagina di accesso di ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ca54abe7fe852b22f2f4d9a717e38d219fa50694
ms.sourcegitcommit: a00fc4426dc4ad0098257f01f0124d6c733d1aef
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/09/2018
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>Modifica il logo della società nella pagina di accesso di ADFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2

#### <a name="change-company-logo"></a>Logo della società modifica  
Per cambiare il logo della società visualizzato nella pagina di accesso, utilizzare la sintassi e il seguente cmdlet PowerShell di Windows PowerShell.  

![modificare il logo](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> È consigliabile usare per il logo di 260 x 350 @ 96 dpi con una dimensione di file non superiore a 10 KB.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> Il `TargetName` parametro è obbligatorio. Il tema predefinito che viene rilasciato con AD FS è denominato *predefinito*.  

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[AD FS Sign-personalizzazione utente](AD-FS-user-sign-in-customization.md)  
