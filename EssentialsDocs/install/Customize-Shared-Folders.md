---
title: Personalizzazione delle cartelle condivise
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47bc4986-14eb-4a29-9930-83a25704a3a0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: bcdd43183512bb225dd4afa916f2782c6eb79d7e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="customize-shared-folders"></a>Personalizzazione delle cartelle condivise

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Per impostazione predefinita, le cartelle del server vengono create nella partizione dati più grande sul disco 0. I partner possono personalizzare il percorso e specificare altre cartelle server utilizzando la procedura seguente:  
  
1.  Usando una configurazione di partizioni personalizzata, creare l'immagine produttore computer e quindi creare una nuova chiave del Registro di sistema di archiviazione prima di utilizzare sysprep. Durante la configurazione iniziale, l'attività di archiviazione IC controlla per questa chiave del Registro di sistema. Se è presente, vengono create le cartelle server predefinite nella directory c:\serverfolders..  
  
    #### <a name="to-create-a-new-storage-registry-key"></a>Per creare una nuova chiave del Registro di sistema di archiviazione  
  
    1.  Nel server, sposta il mouse nell'angolo superiore destro dello schermo e fare clic su **ricerca**.  
  
    2.  Nella casella di ricerca, digitare **regedit**, quindi fare clic su di **Regedit** applicazione.  
  
    3.  Nel riquadro di spostamento, espandere **HKEY_LOCAL_MACHINE**, espandere **SOFTWARE**, quindi espandere **Microsoft**.  
  
    4.  Fare doppio clic su **Windows Server**, fare clic su **New**, quindi fare clic su **chiave**.  
  
    5.  Nome della chiave **archiviazione**.  
  
    6.  Nel riquadro di spostamento, fare doppio clic su Nuovo Storage del Registro di sistema, fare clic **New**, quindi fare clic su **valore DWORD (32 bit)**.  
  
    7.  Nome della stringa **CreateFoldersOnSystem**.  
  
    8.  Fare doppio clic su **CreateFoldersOnSystem**, quindi fare clic su **modifica**. Il **Modifica stringa** viene visualizzata la finestra di dialogo.  
  
    9. Impostare il valore della nuova chiave su **1**, quindi fare clic su **OK**.  
  
2.  Utilizzare lo script PostIC.cmd per spostare le cartelle in un percorso diverso o per creare ulteriori cartelle. Vedere l'esempio seguente: [esempio 1: creare una cartella personalizzata e spostare le cartelle predefinite in un nuovo percorso da PostIC.cmd utilizzando Windows PowerShell](Customize-Shared-Folders.md#BKMK_Example1).  
  
3.  Utilizzare Windows Server Solutions SDK per spostare le cartelle in un percorso diverso o per creare ulteriori cartelle. Vedere l'esempio seguente: [esempio 2: creare una cartella personalizzata e spostare una cartella esistente utilizzando Windows Server Solutions SDK](Customize-Shared-Folders.md#BKMK_Example2).  
  
 Facoltativamente, i partner possono lasciare le cartelle di dati nell'unità C. In questo modo l'utente finale o al rivenditore di stabilire il layout delle cartelle dati sulle unità dati.  
  
###  <a name="BKMK_Example1"></a>Esempio 1: Creare una cartella personalizzata e spostare le cartelle predefinite in un nuovo percorso da PostIC.cmd utilizzando Windows PowerShell  
  
1.  Creare un file PostIC.cmd per eseguire le successive attività di configurazione iniziale come descritto in dettaglio nel [creare il File PostIC.cmd in esecuzione registrazione attività di configurazione iniziale](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md) sezione.  
  
2.  Utilizzando blocco note, creare un file denominato **customizefolders.ps1** nella cartella C:\Windows\Setup\Scripts e quindi incollare Windows PowerShell® i seguenti comandi nel file (deselezionando le righe appropriate in base al comportamento desiderato).  
  
    ```  
    # Move the Documents folder to D:\ServerFolders  
    #Get-WssFolder -name Documents| Move-WssFolder - NewDrive D:\ -Force  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    # Move all folders to D:\ServerFolders  
    #foreach( $folder in Get-WssFolder )  
    #{  
    #    Move-WssFolder $folder -NewDrive D:\ -Force  
    #}  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    # Create a custom folder named Custom Folder  
    #Add-WssFolder -Name CustomFolder -Path D:\ServerFolders\CustomFolder -Description "Custom Folder"  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    exit 0  
    ```  
  
3.  Aggiungere la riga seguente nel file PostIC.cmd per eseguire lo script.  
  
    ```  
    REM Lower the execution policy  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy RemoteSigned"  
  
    REM Execute the folder customization script  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" -NoProfile -Noninteractive -command ". %windir%\setup\scripts\customizefolders.ps1;exit $LASTEXITCODE"  
    Set error_level=%ERRORLEVEL%  
  
    REM Restore the execution policy to deafult  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy Restricted"  
    Set ERRORLEVEL=%error_level%  
    ```  
  
###  <a name="BKMK_Example2"></a>Esempio 2: Creare una cartella personalizzata e spostare una cartella esistente utilizzando Windows Server Solutions SDK  
 Il codice creato può essere compilato come eseguibile e quindi chiamato dal file PostIC.cmd o direttamente da un componente aggiuntivo installato.  
  
```  
static void Main(string[] args)  
{  
 StorageManager storageManager = new StorageManager();  
 //Connect to the StorageManager  
 storageManager.Connect();  
  
 //Move the Documents folder to D:\ServerFolders  
 Folder targetFolder = storageManager.Folders.First(folder => folder.Name == "Documents");  
  
 MoveFolderRequest moveRequest = targetFolder.GetMoveRequest(@"D:\");  
 moveRequest.MoveFolder();  
  
 //Verify operation was successful, if so print result  
 if (moveRequest.Status == OperationStatus.Succeeded)  
 {  
  Console.WriteLine("Folder {0} now located at {1}", targetFolder.Name, targetFolder.Path);  
 }  
  
 string newFolderName = "New Custom Folder";  
 string newFolderLocation = @"C:\ServerFolders\New Custom Folder";  
  
 //Create add request based with specific name and location  
 CreateFolderRequest request = storageManager.GetCreateFolderRequest(newFolderName, newFolderLocation);  
  
 //Give Guest user read only permission to folder  
 request.PermissionsByName.Add(new NamePermission("Guest", Permission.ReadOnly));  
  
 //Create the new folders  
 request.CreateFolder();  
  
 //Verify operation was successful, if so print result  
 if( request.Status == OperationStatus.Succeeded)  
 {  
  Folder newFolder = storageManager.Folders.First(folder => folder.Path == newFolderLocation);  
  
  Console.WriteLine("Folder {0} created at {1}", newFolder.Name, newFolder.Path);  
 }  
}  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)