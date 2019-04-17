---
title: Creazione di un supporto di ripristino Client multilingua
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fdbc016-d464-43cb-bd75-8a63e61588a2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1ad934d297c3092050bd6adbb6bb0f50d1ec6f36
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="build-multi-language-client-restore-media"></a>Creazione di un supporto di ripristino Client multilingua

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

> [!NOTE]
>  È innanzitutto necessario creare un'immagine multilingue di Windows come descritto nel [procedura dettagliata: creazione di immagini multilingue Windows](https://technet.microsoft.com/library/jj126995) prima di aggiungere il language pack di Windows Server Essentials in install.wim.  
  
 Quando si crea il DVD di installazione server multilingua, i language pack verrà installato per install.wim del Server. Le risorse localizzate per il ripristino guidato vengono installate come parte del language pack.  
  
### <a name="to-build-a-multi-language-client-restore-media"></a>Per creare un client multilingua supporto di ripristino  
  
1.  Montare install.wim in c:\mount chiamiamo cartella di c:\mount\Program Files\Windows Server\bin\ClientRestore come radice del supporto di ripristino client: [Radicesupportoripristino] di seguito:  
  
    ```  
    dism /mount-wim /wimfile:install.wim /index:1 /mountdir:c:\mount  
    ```  
  
2.  Montare il file WIM di ripristino client in [RestoreMediaRoot]\Sources\Boot.wim (stessi passaggi devono essere eseguite anche per boot_x86.wim). La riga di comando è:  
  
    ```  
    dism /mount-wim /wimfile:boot.wim /index:1 /mountdir:c:\mountRestore  
    ```  
  
3.  Aggiungi pacchetto WinPE-Setup.cab al supporto di ripristino, eseguendo:  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:WinPE-Setup.cab  
    ```  
  
4.  Utilizzare Blocco note per modificare c:\mountRestore\windows\system32\winpeshl.ini, compilare con il contenuto seguente:  
  
    ```  
    [LaunchApp]  
    AppPath = %SYSTEMDRIVE%\sources\SelectLanguage.exe  
    ```  
  
5.  Aggiungere i language pack per il supporto di ripristino. Aggiungere i language pack può essere eseguita seguente comando:  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:[language pack path]  
    ```  
  
     È necessario aggiungere i seguenti language pack:  
  
    1.  Language pack di WinPE (lp.cab)  
  
    2.  Language pack di WinPE-Setup (WinPE-Setup _ [lingua]. cab, ad esempio, WinPE WinPE-setup_en-US.cab)  
  
    3.  Per i caratteri asiatici, inclusi zh-cn, zh-tw, zh-hk, ko-kr, ja-jp, è necessario installare ulteriore font pack (winpe - fontsupport-[lang]. cab, ad esempio, winpe-fontsupport-WinPE-fontsupport-zh-CN.cab)  
  
6.  Generare nuovo Lang.ini file eseguendo:  
  
    ```  
    dism /image:c:\mountRestore /Gen-LangINI /distribution:mount  
    ```  
  
7.  Eseguire il commit e smontare l'immagine eseguendo:  
  
    ```  
    dism /unmount-wim /mountdir:c:\mountRestore /commit  
    ```  
  
8.  Ripetere il passaggio 2 con il passaggio 7 per [RestoreMediaroot]\Sources\Boot_x86.wim] \sources\boot_x86.wim.  
  
9. Eseguire il commit e smontare l'immagine eseguendo:  
  
    ```  
    dism /unmount-wim /mountdir:c:\mount /commit  
    ```  
  
## <a name="see-also"></a>Vedere anche  

 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)

 [Creazione e personalizzazione dell'immagine](../install/Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](../install/Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](../install/Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](../install/Testing-the-Customer-Experience.md)

