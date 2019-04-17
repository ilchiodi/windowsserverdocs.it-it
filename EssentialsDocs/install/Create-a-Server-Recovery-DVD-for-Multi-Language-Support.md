---
title: Creare un DVD di ripristino Server per il supporto multilingue
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c7da0f6c-9732-4784-9c28-7dad72c4071d
4author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ac547f97b48e4cd0ebf87e0935cadc2c539b4d0b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>Creare un DVD di ripristino Server per il supporto multilingue

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_MLHeadedRecovery"></a>Creare una configurazione del server e il DVD di ripristino server per il supporto multilingue su server amministrato localmente  
  
> [!NOTE]
>  È innanzitutto necessario creare un'immagine multilingue di Windows come descritto nel [procedura dettagliata: creazione di immagini multilingue Windows](https://technet.microsoft.com/library/jj126995) prima di aggiungere il language pack di Windows Server Essentials in install.wim.  
  
 Esistono due fasi dell'installazione: Ambiente preinstallazione di Windows (Windows PE) e la configurazione iniziale. Per impostazione predefinita, non verrà essere visualizzata la pagina Selezione della lingua nella configurazione iniziale.  
  
-   Per un scenario di preinstallazione OEM o un'installazione OEM amministrato da postazione remota, è necessario aggiungere un registro di sistema chiave utilizzando il seguente comando per visualizzare la pagina di selezione della lingua nella configurazione iniziale.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
    > [!IMPORTANT]
    >  Quando gli OEM creano un'immagine in laboratorio, devono scegliere **inglese** come lingua da utilizzare durante la fase Windows PE dell'installazione.  
  
-   Per uno scenario rivenditore opzione Kit (ROK), i clienti ricevono un DVD e, ad esempio, alcuni componenti hardware. Il cliente dovrebbe essere in grado di selezionare la lingua durante l'installazione di Windows PE e la pagina di selezione della lingua non viene più visualizzata durante la configurazione iniziale.  
  
 È possibile scegliere di fornire un unico DVD dual-layer contenente più lingue.  
  
 In questa sezione viene descritto come aggiungere il supporto linguistico a installazione di Windows. Lo strumento principale per la personalizzazione di Windows PE 3.0 è gestione e manutenzione, uno strumento da riga di comando e manutenzione immagini distribuzione. Questa soluzione consente gli scenari seguenti:  
  
1.  Creazione di installazioni multilingue  
  
2.  Creazione di supporti distribuibili  
  
### <a name="prerequisites"></a>Prerequisiti  
 Per aggiungere il supporto multilingua a installazione di Windows, è necessario quanto segue:  
  

-   Un computer tecnico che fornisce tutti gli strumenti e i file di origine necessari per creare un'immagine WinPE personalizzata. Per ulteriori informazioni, vedere [preparare il Computer di riferimento](Prepare-the-Technician-Computer.md).  

-   Un computer tecnico che fornisce tutti gli strumenti e i file di origine necessari per creare un'immagine WinPE personalizzata. Per ulteriori informazioni, vedere [preparare il Computer di riferimento](../install/Prepare-the-Technician-Computer.md).  

  
-   Un DVD di Windows Server Essentials.  
  
-   Windows Server Essentials Language Pack DVD.  
  
###  <a name="BKMK_Steps"></a>Aggiunta del supporto multilingua  
 Per aggiungere il supporto multilingue a installazione di Windows, aggiornare Install.wim aggiungendo Windows Server 2012 e la lingua di Windows Server Essentials Pack.  
  
#### <a name="update-installwim"></a>Aggiornamento di Install.wim  
 In questo passaggio, aggiungere Windows Server 2012 e language pack di Windows Server Essentials in Install.wim.  
  
> [!NOTE]
>  Verificare di installare il language pack per Windows Server 2012. In questo modo si garantisce la corretta personalizzazione. Sono disponibili in Windows Server 2012 Multilingual User Interface Language Pack [Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx). Seguire le istruzioni descritte nel [procedura dettagliata: creazione di immagini multilingue Windows sulla creazione di un multilingue](https://technet.microsoft.com/library/jj126995.aspx) sulla creazione di un'immagine multilingue di Windows prima di aggiungere il language pack di Windows Server Essentials in Install.wim.  
>   
>  Language pack di Windows Server Essentials sono disponibili nei supporti Language pack in \Language Packs\\ < CultureName\ >.  
  
> [!NOTE]
>  Non tutti i language pack potrebbero risultare disponibili prima del rilascio di Windows Server 2012.  
  
###### <a name="to-add-language-packs-to-installwim"></a>Per aggiungere i language pack a Install.wim  
  
1.  Aggiungere language pack del sistema operativo e del prodotto in Install.wim come indicato di seguito (in questo esempio si riferisce al francese):  
  
    ```  
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount  
    Mkdir c:\temp_Scratch  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab  
  
    ```  
  

2.  Aggiungere file specifici della lingua per supportare la creazione di Client Backup USB per il ripristino unità, utilizzando la procedura descritta in flash [il supporto di ripristino Client Build multilingua ](Build-Multi-Language-Client-Restore-Media.md).  

2.  Aggiungere file specifici della lingua per supportare la creazione di Client Backup USB per il ripristino unità, utilizzando la procedura descritta in flash [il supporto di ripristino Client Build multilingua ](../install/Build-Multi-Language-Client-Restore-Media.md).  

  
3.  Ricreare il file Lang.ini nei supporti sfusi in modo da riflettere il supporto linguistico aggiuntivo utilizzando il `DISM /Gen-LangINI`comando, ad esempio:  
  
    ```  
    Dism /image:C:\InstallMount /Gen-LangINI /distribution:C:\my_distribution  
  
    ```  
  
4.  Salvare le modifiche nell'immagine usando il `DISM /unmount /commit`comando, ad esempio:  
  
    ```  
    Dism /Unmount-Wim /MountDir:C:\InstallMount /Commit  
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

