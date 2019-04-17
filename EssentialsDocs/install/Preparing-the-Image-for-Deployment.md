---
title: Preparazione dell'immagine per la distribuzione
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 681c6cad-7fde-494f-86a5-f4c7c15d23f9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 16411ab073e9417c52592aa9a6b13707dd461537
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="preparing-the-image-for-deployment"></a>Preparazione dell'immagine per la distribuzione

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Uno strumento standard per la preparazione di un'immagine è sysprep.exe. Esecuzione di questo strumento consente di generalizzare l'immagine e arresta il server in modo che la configurazione iniziale verrà eseguito quando il server che contiene l'immagine viene riavviato. Tutte le modifiche all'immagine devono essere completate prima di eseguire sysprep.exe.  
  
> [!NOTE]
>  È possibile reimpostare l'attivazione del prodotto Windows un massimo di tre volte utilizzando sysprep.exe.  
  
#### <a name="to-prepare-the-image"></a>Per preparare l'immagine  
  
1.  Eliminare SkipIC.txt aggiunto?  
  
2.  Aprire una finestra del prompt dei comandi con privilegi elevati. Fare clic su **Start**, fare clic destro **prompt dei comandi**e quindi seleziona **Esegui come amministratore**.  
  
3.  Eseguire il comando seguente per reimpostare la chiave del Registro di sistema in modo che l'utente avrà l'intero periodo di tolleranza prima che il server diventi non conforme.  
  
    ```  
    %systemroot%\system32\reg.exe add HKLM\Software\Microsoft\ServerInfrastructureLicensing /v Rearm /t REG_DWORD /d 1 /f  
    ```  
  
4.  Eseguire il comando seguente per aggiungere la chiave del Registro di sistema per visualizzare chiave, pagina della lingua, la pagina delle impostazioni locali e pagina Contratto di licenza. Per impostazione predefinita le pagine non verranno visualizzati durante la configurazione iniziale. Di conseguenza, se si rilascia un prodotto preinstallato, è necessario aggiungere questa chiave del Registro di sistema. Se si sta rilasciando un DVD, è tuttavia consigliabile non aggiungere questa chiave, come queste pagine verranno visualizzate durante la configurazione iniziale e di WinPE.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
5.  Disattiva la configurazione iniziale chiave se la chiave è preinstallata. La pagina con la chiave verrà visualizzata solo se ShowPreinstallPages = true e KeyPreInstalled! = true.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v KeyPreInstalled /t REG_SZ /d true /f  
    ```  
  
6.  Eseguire il comando seguente per aggiungere la chiave del Registro di sistema, se si desidera disabilitare i controlli dei requisiti hardware. Questo è solo per il prodotto preinstallato No che non soddisfa i requisiti hardware. Se si sta rilasciando un DVD o il prodotto soddisfa i requisiti hardware, è consigliabile non aggiungere questa chiave.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
7.  (Facoltativo) Rimuovere i registri in **%programdata%\Microsoft\Windows Server\Logs**.  
  
8.  Preparare il file xml automatico per sysprep come mostrato nel seguente modello.  
  
    ```  
    <unattend xmlns="urn:schemas-microsoft-com:unattend" xmlns:ms="urn:schemas-microsoft-com:asm.v3">  
  
      <settings pass="oobeSystem">  
        <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="https://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  
          <!-- Must have -->  
          <OOBE>  
             <HideEULAPage>true</HideEULAPage>  
          </OOBE>  
          <!-- Must have -->  
          <AutoLogon>   
            <Enabled>true</Enabled>   
            <Username>Administrator</Username>   
            <Domain>.</Domain>   
            <Password>   
              <!--You can set any password you like, but keep it consistent with password settings -->       
              <Value>Admin@123</Value>   
              <PlainText>true</PlainText>   
            </Password>   
          </AutoLogon>   
          <UserAccounts>   
           <AdministratorPassword>   
             <!--You can set any password you like, but keep it consistent with auto logon settings -->       
             <Value>Admin@123</Value>   
             <PlainText>true</PlainText>   
           </AdministratorPassword>   
          </UserAccounts>  
  
          <!-- Optional -->  
          <OEMInformation>  
             <HelpCustomized>true</HelpCustomized>  
             <Manufacturer>OEM name</Manufacturer>  
             <Model>model name</Model>  
             <SupportHours>hours</SupportHours>  
             <SupportPhone>123-456-7890</SupportPhone>  
             <SupportURL>http://www.contoso.com</SupportURL>  
          </OEMInformation>  
  
        </component>  
  
      </settings>  
  
      <settings pass="specialize">  
        <component name="Microsoft-Windows-Shell-Setup" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" processorArchitecture="amd64">  
          <!-- Must have -->  
          <ComputerName>Server</ComputerName>          
          <!-- Optional -->  
          <ProductKey>XXXXX-XXXXX-XXXXX-XXXXX-XXXXX</ProductKey>  
        </component>  
      </settings>  
    </unattend>  
    ```  
  
9. Eseguire il comando seguente per sysprep.  
  
    ```  
    %systemroot%\system32\sysprep\sysprep.exe /generalize /OOBE /unattend:xxx.xml /Quit  
    ```  
  
    > [!IMPORTANT]
    >  È anche possibile aggiungere il unattend.xml in % unitàdisistema % invece di come parametro di sysprep. Se il file si trova in c:\, verrà coperto dalle impostazioni s utente, ma se utilizzato come un parametro di sysprep, verrà coperto dalle impostazioni s utente. Verranno eliminati i unattend.xml in % unitàdisistema % ogni volta che il server viene riavviato. Di conseguenza, assicurarsi che dopo aver creato unattend.xml in % unitàdisistema %, il server non viene riavviato.  
  
10. Eseguire il comando seguente per aggiungere la chiave del Registro di sistema per ignorare pagina della chiave OOBE di Windows.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedProductKey /t REG_DWORD /d 1 /f  
    ```  
  
11. Eseguire il comando seguente per aggiungere la chiave del Registro di sistema per ignorare pagina Seleziona lingua di Windows.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedLanguageSelection /t REG_DWORD /d 1 /f  
    ```  
  
    > [!IMPORTANT]
    >  È necessario eseguire gli ultimi 2 passi, altrimenti la pagina di OOBE di Windows con la quale è scadenza con scenario di amministrazione remota del server di pagina e l'interruzione di configurazione iniziale.  
  
12. Arrestare il prodotto dopo sysprep, è possibile acquisire un'immagine o riavviare il server per continuare la configurazione iniziale da un computer client.  
  
> [!IMPORTANT]
>  I partner che intendono creare i supporti di ripristino server devono acquisire l'immagine e creare i supporti di ripristino prima di procedere al passaggio successivo.