---
title: Creare un DVD di ripristino del server per il supporto multilingue
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.topic: article
ms.assetid: c7da0f6c-9732-4784-9c28-7dad72c4071d
author: daveba
ms.author: daveba
ms.openlocfilehash: 59d8d41e5836ba88b405a058c8340f454b081c06
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2019
ms.locfileid: "69980255"
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>Creare un DVD di ripristino del server per il supporto multilingue

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_MLHeadedRecovery"></a>Creare un DVD di installazione Server e ripristino server per il supporto di più lingue in server amministrati localmente  
  
> [!NOTE]
>  È innanzitutto necessario creare un'immagine multilingue di Windows come descritto nella [procedura dettagliata: Creazione](https://technet.microsoft.com/library/jj126995) di un'immagine multilingue di Windows prima di aggiungere il Language Pack di Windows Server Essentials in install. wim.  
  
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
  
-   Un DVD del Language Pack di Windows Server Essentials.  
  
###  <a name="BKMK_Steps"></a>Aggiunta del supporto per più lingue  
 Per aggiungere il supporto per più lingue a Installazione di Windows si aggiorna il file Install. wim aggiungendo i Language Pack di Windows Server 2012 e Windows Server Essentials.  
  
#### <a name="update-installwim"></a>Aggiornamento di Install.wim  
 In questo passaggio vengono aggiunti i Language Pack di Windows Server 2012 e Windows Server Essentials in install. wim.  
  
> [!NOTE]
>  Verificare di aver installato i Language Pack per Windows Server 2012. In questo modo si garantisce la corretta personalizzazione. I Language Pack dell'interfaccia utente multilingue di Windows Server 2012 sono disponibili in [Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx). Seguire le istruzioni descritte nella [procedura dettagliata: Creazione di un'immagine multilingue di Windows per](https://technet.microsoft.com/library/jj126995.aspx) la creazione di un'immagine multilingue di Windows prima di aggiungere il Language Pack di Windows Server Essentials in install. wim.  
>   
>  I Language Pack di Windows Server Essentials sono disponibili nel supporto dei Language Pack in\\\Language Packs\>< cultureName.  
  
> [!NOTE]
>  Non tutti i Language Pack potrebbero non essere disponibili prima del rilascio di Windows Server 2012.  
  
###### <a name="to-add-language-packs-to-installwim"></a>Per aggiungere i Language Pack a Install.wim  
  
1.  Aggiungere i Language Pack del sistema operativo e del prodotto al file Install.wim secondo la procedura descritta di seguito (l’esempio si riferisce al francese):  
  
    ```  
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount  
    Mkdir c:\temp_Scratch  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab  
  
    ```  
  

2.  Aggiungere file specifici della lingua per supportare la creazione dell'unità flash USB per il ripristino del backup del client usando la procedura descritta in [compilare supporti di ripristino client multilingua](Build-Multi-Language-Client-Restore-Media.md).  

2.  Aggiungere file specifici della lingua per supportare la creazione dell'unità flash USB per il ripristino del backup del client usando la procedura descritta in [compilare supporti di ripristino client multilingua](../install/Build-Multi-Language-Client-Restore-Media.md).  

  
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

