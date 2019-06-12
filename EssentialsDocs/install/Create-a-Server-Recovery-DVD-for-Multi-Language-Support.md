---
title: Creare un DVD di ripristino del server per il supporto multilingue
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
ms.openlocfilehash: e2bbc7bf7af71c671153bf7ba3356ddc08dcc38b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433629"
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>Creare un DVD di ripristino del server per il supporto multilingue

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_MLHeadedRecovery"></a> Creare un programma di installazione server e il DVD di ripristino server per il supporto multilingue su server amministrato localmente  
  
> [!NOTE]
>  È innanzitutto necessario creare un'immagine multilingue di Windows come descritto nel [procedura dettagliata: La creazione di immagini multilingue di Windows](https://technet.microsoft.com/library/jj126995) prima di aggiungere il language pack di Windows Server Essentials in Install. wim.  
  
 La configurazione è composta da due fasi: Ambiente preinstallazione di Windows (Windows PE) e la configurazione iniziale. Per impostazione predefinita, la pagina per la selezione della lingua non viene visualizzata nella configurazione iniziale.  
  
- Per uno scenario con una preinstallazione OEM o un'installazione OEM gestita in remoto, è necessario aggiungere una chiave del Registro di sistema tramite il seguente comando per visualizzare la pagina di selezione della lingua nella configurazione iniziale.  
  
  ```  
  %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
  ```  
  
  > [!IMPORTANT]
  >  Quando gli OEM creano un’immagine in laboratorio, devono scegliere **Inglese** come lingua da utilizzare durante la fase di installazione di Windows PE.  
  
- In uno scenario relativo a Reseller Option Kit (ROK), i clienti ricevono un DVD e, talvolta, alcuni componenti hardware. Il cliente dovrebbe essere in grado di selezionare la lingua durante la configurazione di Windows PE e la pagina di selezione della lingua non viene più visualizzata durante la configurazione iniziale.  
  
  È possibile fornire un unico DVD dual-layer contenente più lingue.  
  
  In questa sezione viene descritto come aggiungere il supporto linguistico a Installazione di Windows. Lo strumento principale per la personalizzazione di Windows PE 3.0 è Gestione e manutenzione immagini distribuzione, uno strumento da riga di comando. Questa soluzione rende possibili gli scenari seguenti:  
  
1.  Creazione di installazioni multilingue  
  
2.  Creazione di supporti distribuibili  
  
### <a name="prerequisites"></a>Prerequisiti  
 Per aggiungere il supporto multilingue a Installazione di Windows, è necessario disporre dei seguenti elementi:  
  

-   Un computer tecnico in cui siano disponibili tutti gli strumenti e i file di origine necessari per creare un’immagine WinPE personalizzata. Per altre informazioni, vedere [Prepare the Technician Computer](Prepare-the-Technician-Computer.md).  

-   Un computer tecnico in cui siano disponibili tutti gli strumenti e i file di origine necessari per creare un’immagine WinPE personalizzata. Per altre informazioni, vedere [Prepare the Technician Computer](../install/Prepare-the-Technician-Computer.md).  

  
-   Un DVD di Windows Server Essentials.  
  
-   Windows Server Essentials Language Pack DVD.  
  
###  <a name="BKMK_Steps"></a> Aggiunta del supporto multilingua  
 Per aggiungere il supporto multilingue a installazione di Windows è aggiornare il file Install. wim mediante l'aggiunta di Windows Server 2012 e la lingua di Windows Server Essentials Pack ad esso.  
  
#### <a name="update-installwim"></a>Aggiornamento di Install.wim  
 In questo passaggio si aggiungere Windows Server 2012 e language pack di Windows Server Essentials in Install. wim.  
  
> [!NOTE]
>  Verificare di aver installato i language pack per Windows Server 2012. In questo modo si garantisce la corretta personalizzazione. Windows Server 2012 Multilingual User Interface Language Pack sono disponibili nel [Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx). Seguire le istruzioni descritte nel [procedura dettagliata: La creazione di immagini multilingue di Windows sulla creazione di un multilingue](https://technet.microsoft.com/library/jj126995.aspx) sulla creazione di un'immagine multilingue di Windows prima di aggiungere il language pack di Windows Server Essentials in Install. wim.  
>   
>  Language pack di Windows Server Essentials sono disponibili nel Language pack in \language Pack \Language\\< CultureName\>.  
  
> [!NOTE]
>  Non tutti i language pack potrebbero risultare disponibili prima della versione di Windows Server 2012.  
  
###### <a name="to-add-language-packs-to-installwim"></a>Per aggiungere i Language Pack a Install.wim  
  
1.  Aggiungere i Language Pack del sistema operativo e del prodotto al file Install.wim secondo la procedura descritta di seguito (l’esempio si riferisce al francese):  
  
    ```  
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount  
    Mkdir c:\temp_Scratch  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab  
  
    ```  
  

2.  Aggiungere file specifici del linguaggio per supportare la creazione di Client Backup Restore USB flash drive, tramite la procedura descritta in [supporti di ripristino Client compilare multilingue](Build-Multi-Language-Client-Restore-Media.md).  

2.  Aggiungere file specifici del linguaggio per supportare la creazione di Client Backup Restore USB flash drive, tramite la procedura descritta in [supporti di ripristino Client compilare multilingue](../install/Build-Multi-Language-Client-Restore-Media.md).  

  
3.  Ricreare il file Lang.ini nei supporti sfusi in modo da riflettere il supporto linguistico aggiuntivo utilizzando il comando `DISM /Gen-LangINI` . Ad esempio:  
  
    ```  
    Dism /image:C:\InstallMount /Gen-LangINI /distribution:C:\my_distribution  
  
    ```  
  
4.  Salvare le modifiche nell’immagine utilizzando il comando `DISM /unmount /commit` . Ad esempio:  
  
    ```  
    Dism /Unmount-Wim /MountDir:C:\InstallMount /Commit  
    ```  
  
## <a name="see-also"></a>Vedere anche  

 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)

 [Creazione e personalizzazione dell'immagine](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](../install/Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](../install/Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](../install/Testing-the-Customer-Experience.md)

