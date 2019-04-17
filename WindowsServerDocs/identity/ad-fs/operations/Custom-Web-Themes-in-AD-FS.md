---
ms.assetid: 0379abc3-25c7-46ab-9a6b-80a5152365b0
title: Temi Web personalizzati in ADFS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 300c9fda84285ddfc52a4f47ea0198deb6fd33ef
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="custom-web-themes-in-ad-fs"></a>Temi Web personalizzati in ADFS 

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Il tema è spedito out-of\-the\-box è denominato Default. È possibile esportare il tema predefinito e usarlo in modo che è possibile avviare rapidamente. È possibile personalizzare l'aspetto e comportamento, incluso il layout, modificando il file CSS, importare e applicare il nuovo tema e quindi è possibile utilizzare la personalizzazione dell'aspetto e il comportamento. Utilizzando il file CSS rende anche più facile lavorare con strumenti di progettazione web.  
  
Il cmdlet seguente crea un tema web personalizzato duplicando il tema web predefinito.  
  
  
`New-AdfsWebTheme –Name custom –SourceName default ` 

  
È possibile modificare il file CSS e configurare il nuovo tema web usando il nuovo file CSS. Per esportare un tema web, usare il cmdlet seguente.  
  

    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  

  
Per applicare il file CSS per il nuovo tema, usare il cmdlet seguente.  
  

    Set-AdfsWebTheme –TargetName custom –StyleSheet @{path=”c:\NewTheme.css”}  
  
  
Il cmdlet seguente crea un tema web personalizzato da un nuovo foglio di stile.  
  
  
`New-AdfsWebTheme –Name custom –StyleSheet @{path=”c:\NewTheme.css”} –RTLStyleSheetPath c:\NewRtlTheme.css ` 
  
  
  
Per applicare il tema web personalizzato per ADFS, usare il cmdlet seguente.  
  

`Set-AdfsWebConfig -ActiveThemeName custom`  

  
Per aggiungere JavaScript ad ADFS, usare il cmdlet seguente.  
  
 
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’ /adfs/portal/script/onload.js’;path="D:\inetpub\adfsassets\script\onload.js"}  


## <a name="additional-references"></a>Riferimenti aggiuntivi 
[AD FS Sign-personalizzazione utente](AD-FS-user-sign-in-customization.md)  
