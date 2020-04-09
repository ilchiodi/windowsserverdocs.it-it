---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: Modificare il nome della società nella pagina di accesso AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 378979825c0e8e505c3996cf8cbcdb471a0c7d79
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859964"
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>Modificare il nome della società nella pagina di accesso AD FS
 
Per modificare il nome della società visualizzato nella pagina Sign\-in, usare la sintassi e il cmdlet di Windows PowerShell seguenti. Questo valore è configurato per impostazione predefinita in base al valore del nome visualizzato del Servizio federativo specificato durante l'installazione.  

![modificare il nome](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> È anche possibile usare l'ambiente di scripting integrato di Windows PowerShell \(ISE\) per modificare il nome della società. Utilizzando la Windows PowerShell ISE, è possibile visualizzare il contenuto in un ambiente conforme a Unicode\-. Per altre informazioni, vedere [Introduzione a Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md)  
  
