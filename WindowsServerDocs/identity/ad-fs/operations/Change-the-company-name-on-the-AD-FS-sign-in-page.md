---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: Modificare il nome della società nella pagina di accesso di AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1711e7d7de871c9ae9b1b7ea7b21f6e75ae15220
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868472"
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>Modificare il nome della società nella pagina di accesso di AD FS

>Si applica a: Windows Server 2016, Windows Server 2012 R2
 
Per modificare il nome della società che viene visualizzato il segno\-nella pagina, utilizzare la sintassi e i cmdlet di Windows PowerShell seguente. Questo valore è configurato per impostazione predefinita in base al valore del nome visualizzato del Servizio federativo specificato durante l'installazione.  

![Modifica nome](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> È anche possibile usare Windows PowerShell Integrated Scripting Environment \(ISE\) per modificare il nome della società. Usando Windows PowerShell ISE, è possibile visualizzare il contenuto in un formato Unicode\-ambiente conforme. Per altre informazioni, vedere [Introduzione a Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Altri riferimenti 
[AD FS Sign-in personalizzazione dell'utente](AD-FS-user-sign-in-customization.md)  
  
