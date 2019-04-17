---
ms.assetid: c89a977c-b09f-44ec-be42-41e76a6cf3ad
title: Rimuovere il copyright Microsoft
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c2e6f9445e53a5b5867a763d58ad4a6ca3600cbe
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="remove-the-microsoft-copyright"></a>Rimuovere il copyright Microsoft 

>Si applica a: Windows Server 2016, Windows Server 2012 R2
 
Per impostazione predefinita, le pagine di ADFS includono il copyright Microsoft. Per rimuoverlo dalle pagine personalizzate, è possibile utilizzare la procedura seguente. 

![Rimuovere il copyright](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png) 
  
## <a name="to-remove-the-microsoft-copyright"></a>Per rimuovere il copyright Microsoft  
  
1.  Creare un tema personalizzato basato sul valore predefinito.  
  

    `New-AdfsWebTheme –Name custom –SourceName default ` 
 
  
2.  Esportare il tema specificando la cartella di output.  

    `Export-AdfsWebTheme -Name custom -DirectoryPath C:\customWebTheme ` 

  
3.  Individuare il file Style.css che si trova nella cartella di output. Utilizzando l'esempio precedente, il percorso sarà C:\\CustomWebTheme\\Css\\Style.css.  
  
4.  Aprire il file Style.css con un editor, ad esempio Blocco note.  
  
5.  Individuare il `#copyright` parte e modificarla come segue:  
  `#copyright {color:#696969; display:none;} ` 
 
6.  Creare un tema personalizzato basato sul nuovo file Style.css.  
  
    `Set-AdfsWebTheme -TargetName custom -StyleSheet @{locale="";path="C:\customWebTheme\css\style.css"}  `

7.  Attivare il nuovo tema.  
  

    `Set-AdfsWebConfig -ActiveThemeName custom ` 


A questo punto, non dovresti più visualizzare il copyright nella parte inferiore della pagina di accesso.

![Rimuovere il copyright](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1a.png) 

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[AD FS Sign-personalizzazione utente](AD-FS-user-sign-in-customization.md) 
