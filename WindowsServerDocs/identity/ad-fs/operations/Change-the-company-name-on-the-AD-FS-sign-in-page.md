---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: "Modificare il nome della società nella pagina di accesso di ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8e99f6fed8922ed15bf78a98b207b6f46767763a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>Modificare il nome della società nella pagina di accesso di ADFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2
 
Per modificare il nome della società visualizzato nella pagina di accesso, usare la sintassi e i cmdlet PowerShell di Windows PowerShell seguente. Questo valore è impostato per impostazione predefinita utilizzando il valore dal nome visualizzato del servizio federativo specificato durante l'installazione.  

![modificare nome](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> È inoltre possibile utilizzare \(ISE\) di Windows PowerShell Integrated Scripting Environment per modificare il nome della società. Utilizzando Windows PowerShell ISE, è possibile visualizzare il contenuto in un ambiente conforme a familiare. Per ulteriori informazioni, vedere [Introduzione a Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[AD FS Sign-personalizzazione utente](AD-FS-user-sign-in-customization.md)  
  
