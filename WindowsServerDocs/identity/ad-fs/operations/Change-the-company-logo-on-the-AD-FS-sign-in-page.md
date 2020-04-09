---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: Modifica del logo della società nella pagina di accesso AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d12429b22265495cb8168ce3e5993a5cf3e74a0c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859974"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>Modifica del logo della società nella pagina di accesso AD FS

#### <a name="change-company-logo"></a>Cambiare il logo della società  
Per modificare il logo della società visualizzato nella pagina Sign\-in, usare la sintassi e il cmdlet di Windows PowerShell per PowerShell seguenti.  

![cambia logo](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> È consigliabile usare un logo con dimensioni di 260x35, 96 dpi, in un file con dimensioni massime di 10 KB.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> Il parametro `TargetName` è obbligatorio. Il tema predefinito rilasciato con AD FS è denominato *default*.  

## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md)  
