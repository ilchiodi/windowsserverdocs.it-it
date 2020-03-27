---
title: Personalizzazione delle cartelle condivise
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47bc4986-14eb-4a29-9930-83a25704a3a0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 387f9570e87bd2bd65266489b0f3eac6c945e3be
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311911"
---
# <a name="customize-shared-folders"></a>Personalizzazione delle cartelle condivise

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Per impostazione predefinita, le cartelle server vengono create sulla partizione dati più grande sul Disco 0. I partner possono personalizzare il percorso e specificare altre cartelle server utilizzando la seguente procedura:  
  
1. Utilizzando una configurazione di partizioni personalizzata, creare l'immagine produttore computer e la nuova chiave Storage del Registro di sistema prima di utilizzare sysprep. Durante la configurazione iniziale, l'attività di configurazione iniziale relativa all'archiviazione verifica la presenza di questa chiave del Registro di sistema. Se è presente, vengono create le cartelle server predefinite nella directory C:\ServerFolders.  
  
   #### <a name="to-create-a-new-storage-registry-key"></a>Creazione di una nuova chiave Storage del Registro di sistema  
  
   1.  Sul server, spostare il puntatore del mouse verso l'angolo superiore destro dello schermo e fare clic su **Trova**.  
  
   2.  Nella casella di ricerca digitare **regedit** e quindi fare clic sull'applicazione **Regedit**.  
  
   3.  Nel riquadro di spostamento, espandere **HKEY_LOCAL_MACHINE**, poi espandere **SOFTWARE**, quindi espandere **Microsoft**.  
  
   4.  Fare clic con il pulsante destro del mouse su **Windows Server**, fare clic su **Nuovo**, quindi fare clic su **Chiave**.  
  
   5.  Assegnare alla chiave il nome **Storage**.  
  
   6.  Nel riquadro di spostamento, fare clic con il pulsante destro del mouse sulla nuova chiave di registro Storage, fare clic su **Nuovo**, quindi fare clic su **Valore DWORD (32 bit)** .  
  
   7.  Assegnare alla stringa il nome **CreateFoldersOnSystem**.  
  
   8.  Fare clic con il pulsante destro del mouse su **CreateFoldersOnSystem**, quindi fare clic su **Modifica**. Verrà visualizzata la finestra di dialogo **Modifica stringa**.  
  
   9. Impostare il valore della nuova chiave su **1**, quindi fare clic su **OK**.  
  
2. Utilizzare lo script PostIC.cmd per spostare le cartelle in un percorso diverso o per creare ulteriori cartelle. Vedere il seguente esempio: [Esempio 1 - Creazione di una cartella personalizzata e spostamento delle cartelle predefinite in un nuovo percorso da PostIC.cmd utilizzando Windows PowerShell](Customize-Shared-Folders.md#BKMK_Example1).  
  
3. Utilizzare Windows Server Solutions SDK per spostare le cartelle in un percorso diverso o per creare ulteriori cartelle. Vedere il seguente esempio: [Esempio 2 - Creazione di una cartella personalizzata e spostamento di una cartella esistente utilizzando Windows Server Solutions SDK](Customize-Shared-Folders.md#BKMK_Example2).  
  
   Facoltativamente, i partner possono lasciare le cartelle dati sull'unità C. In questo modo si permette all'utente finale o al rivenditore di stabilire il layout delle cartelle dati sulle unità dati.  
  
###  <a name="example-1-create-a-custom-folder-and-move-the-default-folders-to-a-new-location-from-posticcmd-by-using-windows-powershell"></a><a name="BKMK_Example1"></a>Esempio 1: creare una cartella personalizzata e spostare le cartelle predefinite in un nuovo percorso da post-. cmd usando Windows PowerShell  
  
1.  Creare un file PostIC.cmd per eseguire le attività successive alla configurazione iniziale come illustrato nella sezione [Creazione del file PostIC.cmd per eseguire le attività successive alla configurazione iniziale](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md).  
  
2.  Con Blocco note, creare un file denominato **customizefolders.ps1** nella cartella C:\Windows\Setup\Scripts, quindi incollare nel file i seguenti comandi di Windows PowerShell® (deselezionando le righe appropriate in base al comportamento desiderato).  
  
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
  
3.  Aggiungere la seguente riga nel file PostIC.cmd per eseguire lo script.  
  
    ```  
    REM Lower the execution policy  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy RemoteSigned"  
  
    REM Execute the folder customization script  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" -NoProfile -Noninteractive -command ". %windir%\setup\scripts\customizefolders.ps1;exit $LASTEXITCODE"  
    Set error_level=%ERRORLEVEL%  
  
    REM Restore the execution policy to default  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy Restricted"  
    Set ERRORLEVEL=%error_level%  
    ```  
  
###  <a name="example-2-create-a-custom-folder-and-move-an-existing-folder-by-using-the-windows-server-solutions-sdk"></a><a name="BKMK_Example2"></a>Esempio 2: creare una cartella personalizzata e spostare una cartella esistente usando Windows Server Solutions SDK  
 Il codice creato può essere compilato come eseguibile e quindi richiamato dal file PostIC.cmd oppure direttamente da un componente aggiuntivo installato.  
  
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
  
## <a name="see-also"></a>Vedi anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)
