---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: Modificare il nome della società nella pagina di accesso AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c4991b27f104cb96f55f09fa9467f2b93868b910
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407732"
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>Modificare il nome della società nella pagina di accesso AD FS
 
Per modificare il nome della società visualizzato nella pagina Sign @ no__t-0cm, usare la sintassi e il cmdlet di Windows PowerShell seguenti. Questo valore è configurato per impostazione predefinita in base al valore del nome visualizzato del Servizio federativo specificato durante l'installazione.  

![modificare il nome](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> Per modificare il nome della società, è anche possibile usare l'ambiente di scripting integrato di Windows PowerShell \(ISE @ no__t-1. Utilizzando la Windows PowerShell ISE, è possibile visualizzare il contenuto in un ambiente Unicode @ no__t-0compliant. Per altre informazioni, vedere [Introduzione a Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md)  
  
