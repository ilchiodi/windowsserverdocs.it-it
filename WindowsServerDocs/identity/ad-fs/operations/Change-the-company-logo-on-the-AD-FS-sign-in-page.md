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
ms.openlocfilehash: 6e0271ae3d7ac120e510a2fb81fb55c8d10b3c87
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813582"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>Modificare il logo della società nella pagina di accesso di AD FS

>Si applica a: Windows Server 2016, Windows Server 2012 R2

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
