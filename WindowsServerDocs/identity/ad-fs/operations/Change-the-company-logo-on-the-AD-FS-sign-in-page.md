---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: Modificare il logo della società nella pagina di accesso di AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fe5c138466ea288b5dfb8c7c284603150ab9d874
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190034"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>Modificare il logo della società nella pagina di accesso di AD FS

#### <a name="change-company-logo"></a>Cambiare il logo della società  
Per modificare il logo della società che viene visualizzato il segno\-nella pagina, utilizzare la sintassi e i cmdlet di PowerShell di Windows PowerShell seguente.  

![modificare il logo](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> È consigliabile usare un logo con dimensioni di 260x35, 96 dpi, in un file con dimensioni massime di 10 KB.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> Il parametro `TargetName` è obbligatorio. Il tema predefinito incluso in AD FS è denominato *predefinito*.  

## <a name="additional-references"></a>Altri riferimenti 
[AD FS Sign-in personalizzazione dell'utente](AD-FS-user-sign-in-customization.md)  
