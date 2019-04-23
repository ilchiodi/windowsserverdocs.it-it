---
ms.assetid: 0379abc3-25c7-46ab-9a6b-80a5152365b0
title: Temi Web personalizzati in ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 300c9fda84285ddfc52a4f47ea0198deb6fd33ef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882292"
---
# <a name="custom-web-themes-in-ad-fs"></a>Temi Web personalizzati in ADFS 

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Il tema è spedito\-di\-di\-casella è denominata Default. Si può esportare il tema predefinito e usarlo come base per iniziare rapidamente. È possibile personalizzare l'aspetto e il comportamento, incluso il layout, modificando il file con estensione CSS, importare e applicare il nuovo tema e quindi usare l'aspetto e il comportamento personalizzati. L'uso del file CSS rende anche più facile lavorare con le finestre di progettazione Web.  
  
Il seguente cmdlet crea un tema Web personalizzato duplicando il tema Web predefinito.  
  
  
`New-AdfsWebTheme –Name custom –SourceName default ` 

  
È possibile modificare il file CSS e configurare il nuovo tema Web usando il nuovo file CSS. Per esportare un tema Web, usare il cmdlet seguente.  
  

    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  

  
Per applicare il file CSS al nuovo tema Web, usare il cmdlet seguente.  
  

    Set-AdfsWebTheme –TargetName custom –StyleSheet @{path=”c:\NewTheme.css”}  
  
  
Il cmdlet seguente crea un tema Web personalizzato da un nuovo foglio di stile.  
  
  
`New-AdfsWebTheme –Name custom –StyleSheet @{path=”c:\NewTheme.css”} –RTLStyleSheetPath c:\NewRtlTheme.css ` 
  
  
  
Per applicare il tema web personalizzato per ADFS, utilizzare il cmdlet seguente.  
  

`Set-AdfsWebConfig -ActiveThemeName custom`  

  
Per aggiungere JavaScript ad ADFS, utilizzare il cmdlet seguente.  
  
 
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’ /adfs/portal/script/onload.js’;path="D:\inetpub\adfsassets\script\onload.js"}  


## <a name="additional-references"></a>Altri riferimenti 
[AD FS Sign-in personalizzazione dell'utente](AD-FS-user-sign-in-customization.md)  
