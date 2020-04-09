---
title: Preparazione dell'immagine per la distribuzione
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 681c6cad-7fde-494f-86a5-f4c7c15d23f9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8ef4cb775f6d73f94a47d5d86ca235d459dd4b10
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819904"
---
# <a name="preparing-the-image-for-deployment"></a>Preparazione dell'immagine per la distribuzione

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Uno strumento standard per la preparazione di un'immagine è sysprep.exe. Quando si esegue questo strumento, l'immagine viene generalizzata e il server viene spento, in modo tale che, al riavvio del server che contiene l'immagine, venga eseguita la configurazione iniziale. Tutte le modifiche all'immagine devono essere completate prima di eseguire sysprep.exe.  
  
> [!NOTE]
>  È possibile reimpostare l'attivazione del prodotto Windows al massimo tre volte utilizzando sysprep.exe.  
  
#### <a name="to-prepare-the-image"></a>Per preparare l'immagine  
  
1.  Eliminare il file SkipIC.txt aggiunto.  
  
2.  Aprire una finestra del prompt dei comandi con privilegi elevati. Fare clic sul pulsante **Start**, fare clic con il pulsante destro del mouse su **Prompt dei comandi**, quindi scegliere **Esegui come amministratore**.  
  
3.  Utilizzare il seguente comando per reimpostare la chiave del Registro di sistema in modo che l'utente abbia a disposizione l'intero periodo di tolleranza prima che il server diventi non conforme.  
  
    ```  
    %systemroot%\system32\reg.exe add HKLM\Software\Microsoft\ServerInfrastructureLicensing /v Rearm /t REG_DWORD /d 1 /f  
    ```  
  
4.  Per aggiungere la chiave del Registro di sistema per visualizzare la chiave, la pagina della lingua, la pagina delle impostazioni locali e la pagina del Contratto di Licenza con l'utente finale, utilizzare il seguente comando. Per impostazione predefinita, queste pagine non verranno visualizzate durante la configurazione iniziale. Di conseguenza, se si rilascia un prodotto preinstallato, è necessario aggiungere questa chiave del Registro di sistema. Tuttavia, se si sta rilasciando un DVD, è opportuno evitare di aggiungere questa chiave in quanto queste pagine verranno visualizzate durante la configurazione iniziale e di WinPE.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
5.  Disabilitare la pagina con la chiave per la configurazione iniziale se per il prodotto la chiave è preinstallata. La pagina con la chiave verrà visualizzata solo se ShowPreinstallPages = true e KeyPreInstalled != true.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v KeyPreInstalled /t REG_SZ /d true /f  
    ```  
  
6.  Utilizzare il seguente comando per aggiungere la chiave del Registro di sistema se si desidera disabilitare i controlli dei requisiti hardware. Questa soluzione è consigliabile solo se il prodotto preinstallato no non soddisfa i requisiti hardware. Se si fornisce un DVD o il prodotto soddisfa i requisiti hardware, è preferibile non aggiungere questa chiave.  
  
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
  
9. Utilizzare il seguente comando per sysprep.  
  
    ```  
    %systemroot%\system32\sysprep\sysprep.exe /generalize /OOBE /unattend:xxx.xml /Quit  
    ```  
  
    > [!IMPORTANT]
    >  È anche possibile aggiungere il file unattend.xml in %unitàdisistema% invece di un parametro di sysprep. Se il file si trova in c:\ verrà analizzato dalle impostazioni dell'utente, ma se usato come parametro di Sysprep, non verrà coperto dalle impostazioni dell'utente. Il file unattend.xml in %unitàdisistema% viene eliminato ogni volta che il server viene riavviato. Pertanto, verificare che dopo la creazione del file unattend.xml in %unitàdisistema%, il server non venga riavviato.  
  
10. Utilizzare il seguente comando per aggiungere la chiave del Registro di sistema per ignorare la pagina della chiave OOBE di Windows.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedProductKey /t REG_DWORD /d 1 /f  
    ```  
  
11. Utilizzare il seguente comando per aggiungere la chiave del Registro di sistema per ignorare la pagina di selezione della lingua di Windows.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedLanguageSelection /t REG_DWORD /d 1 /f  
    ```  
  
    > [!IMPORTANT]
    >  È necessario eseguire gli ultimi 2 passi, altrimenti la pagina di OOBE di Windows per la configurazione iniziale verrà visualizzata come è normale che sia, alterando però l'intero scenario di amministrazione remota del server.  
  
12. Arrestare il prodotto dopo sysprep; è quindi possibile acquisire un'immagine oppure riavviare il server per continuare con la configurazione iniziale da un computer client.  
  
> [!IMPORTANT]
>  I partner che intendono creare i supporti di ripristino del server devono acquisire l'immagine e creare i supporti di ripristino prima di proseguire con il passaggio successivo.