---
title: Creare un supporto di ripristino client multilingue
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 2fdbc016-d464-43cb-bd75-8a63e61588a2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c47b77e30aafdf6d882bb0d6af4777c7417acba9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817344"
---
# <a name="build-multi-language-client-restore-media"></a>Creare un supporto di ripristino client multilingue

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

> [!NOTE]
>  È innanzitutto necessario creare un'immagine multilingue di Windows come descritto nella [procedura dettagliata: creazione di un'immagine multilingue di Windows](https://technet.microsoft.com/library/jj126995) prima di aggiungere il Language Pack di Windows Server Essentials in install. wim.  
  
 Quando si crea un DVD per l'installazione del server multilingua, i Language Pack vengono installati per install.wim del server. Le risorse localizzate per il ripristino guidato vengono installate come parte del Language Pack.  
  
### <a name="to-build-a-multi-language-client-restore-media"></a>Per creare un supporto di ripristino client multilingua  
  
1.  Montare install.wim in c:\mount e poi definire la cartella c:\mount\Program Files\Windows Server\bin\ClientRestore come radice del supporto di ripristino client [RadiceSupportoRipristino] di seguito:  
  
    ```  
    dism /mount-wim /wimfile:install.wim /index:1 /mountdir:c:\mount  
    ```  
  
2.  Montare il file WIM per il ripristino client in [RadiceSupportoRipristino]\Sources\Boot.wim (seguire la stessa procedura per boot_x86.wim). La riga di comando è:  
  
    ```  
    dism /mount-wim /wimfile:boot.wim /index:1 /mountdir:c:\mountRestore  
    ```  
  
3.  Aggiungere il pacchetto WinPE-Setup.cab al supporto di ripristino eseguendo:  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:WinPE-Setup.cab  
    ```  
  
4.  Utilizzare Blocco note per modificare c:\mountRestore\windows\system32\winpeshl.ini inserendo il seguente contenuto:  
  
    ```  
    [LaunchApp]  
    AppPath = %SYSTEMDRIVE%\sources\SelectLanguage.exe  
    ```  
  
5.  Aggiungere i Language Pack al supporto di ripristino. Per aggiungere i Language Pack, utilizzare il seguente comando:  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:[language pack path]  
    ```  
  
     Non è necessario aggiungere i seguenti Language Pack:  
  
    1.  Language Pack di WinPE (lp.cab)  
  
    2.  Language Pack di WinPE-Setup (WinPE-Setup_[lingua].cab, ad esempio, WinPE-Setup_en-us.cab)  
  
    3.  Per i caratteri asiatici, inclusi zh-cn, zh-tw, zh-hk, ko-kr, ja-jp, è necessario installare un ulteriore Font Pack (winpe-fontsupport-[lang].cab, ad esempio, winpe-fontsupport-zh-cn.cab)  
  
6.  Generare il nuovo file Lang.ini file eseguendo:  
  
    ```  
    dism /image:c:\mountRestore /Gen-LangINI /distribution:mount  
    ```  
  
7.  Eseguire il commit e smontare l'immagine eseguendo:  
  
    ```  
    dism /unmount-wim /mountdir:c:\mountRestore /commit  
    ```  
  
8.  Ripetere i passi da 2 a 7 per [RadiceSupportoRipristino]\Sources\Boot_x86.wim.  
  
9. Eseguire il commit e smontare l'immagine eseguendo:  
  
    ```  
    dism /unmount-wim /mountdir:c:\mount /commit  
    ```  
  
## <a name="see-also"></a>Vedi anche  

 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)

 [Creazione e personalizzazione dell'immagine](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](../install/Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](../install/Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](../install/Testing-the-Customer-Experience.md)

