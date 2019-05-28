---
title: Funzionalità su richiesta di compatibilità dell'app Server Core
description: Come installare funzionalità di Windows Server su richiesta
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 05/24/2019
ms.openlocfilehash: c9af38720df79918bed3404995e81a7f93a10744
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222891"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>Funzionalità su richiesta di compatibilità dell'app Server Core

> Si applica a: Windows Server 2019, canale semestrale di Windows Server

Il **Server Core App compatibilità funzionalità su richiesta** è un pacchetto di funzionalità facoltativa che può essere aggiunto per le installazioni Server Core di Windows Server 2019 o canale semestrale di Windows Server, in qualsiasi momento.

Per altre informazioni sulle funzionalità su richiesta (DOM), vedere [funzionalità su richiesta](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).

## <a name="why-install-the-app-compatibility-fod"></a>Il motivo per cui installare il DOM di compatibilità delle App?

Compatibilità dell'applicazione, una funzionalità su richiesta per Server Core, migliora notevolmente la compatibilità dell'applicazione dell'opzione di installazione di Windows Server Core, includendo un subset di file binari e i pacchetti da Windows Server con esperienza Desktop, senza l'aggiunta di Ambiente con interfaccia grafica di esperienza Desktop di Windows Server. Questo pacchetto facoltativo è disponibile su un'immagine ISO separata o da Windows Update, ma può essere aggiunti solo alle immagini e le installazioni di Windows Server Core.

I due valori primari che fornisce il DOM di compatibilità delle App sono:

- Aumenta la compatibilità di Server Core per le applicazioni server che sono già nel mercato o già sviluppato da organizzazioni e distribuito.
- Collabora con la fornitura di componenti del sistema operativo e una maggiore compatibilità delle applicazioni software dell'uso di strumenti acuta risoluzione dei problemi e gli scenari di debug.

Componenti del sistema operativo che sono disponibili come parte dei Server Core App compatibilità DOM includono:

-   Microsoft Management Console (mmc.exe)

-   Visualizzatore eventi (Eventvwr. msc)

-   Performance Monitor (PerfMon.exe)

-   Monitoraggio risorse (Resmon.exe)

-   Gestione di dispositivi (devmgmt. msc)

-   Esplora file (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   Gestione disco (diskmgmt. msc)

-   Gestione Cluster di failover (Cluadmin)

    -   Richiede l'aggiunta della funzionalità del Server Windows di Clustering di Failover prima di tutto.

        -   Da una sessione di PowerShell con privilegi elevata: 

            ```PowerShell
            Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools
            ```

        -   Per eseguire Gestione Cluster di Failover, immettere **cluadmin** al prompt dei comandi.

I server che esegue Windows Server, 1903 e versioni successive supportano anche i componenti seguenti:

- Gestione di Hyper-V (virtmgmt.msc)
- Utilità di pianificazione (Taskschd. msc)

## <a name="installing-the-app-compatibility-fod"></a>Installare la compatibilità delle applicazioni DOM

Il DOM di compatibilità delle App può essere installato solo in Server Core. Non tentare di aggiungere Server Core App compatibilità DOM per un'installazione di Windows Server di Windows Server con esperienza Desktop. I pacchetti facoltativi DOM stesso ISO possono essere utilizzati per le installazioni Server Core di Windows Server 2019 oppure le installazioni di canale semestrale di Windows Server.

1. Se il server possa connettersi a Windows Update, tutto ciò che devi fare è eseguire il comando seguente da una sessione di PowerShell con privilegi elevata e quindi riavviare il Server di Windows dopo il completamento del comando in esecuzione:

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0
    ```

2. Se il server non è possibile connettersi a Windows Update, invece scaricare i pacchetti facoltativi Server DOM ISO e copiare l'immagine ISO a una cartella condivisa nella rete locale:

   - Se si dispone di un contratto multilicenza è possibile scaricare il file di immagine ISO DOM Server dallo stesso portale in cui viene ottenuto il file di immagine ISO del sistema operativo: [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
   - Il file di immagine ISO DOM Server è disponibile anche nella [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server) o scegliere il [portale di Visual Studio](https://visualstudio.microsoft.com) per i sottoscrittori.

3. Accedere con un account di amministratore nel computer Server Core che è connesso alla rete locale e che si desidera aggiungere al DOM.

4. Usare **net usare**, o un altro metodo, per connettersi al percorso del FOD ISO.

5. Copiare FOD ISO in una cartella locale di propria scelta.

6. Montare l'ISO FOD usando il comando seguente in una sessione di PowerShell con privilegi elevata:

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

7. Eseguire il comando seguente:

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0 -Source <Mounted_Server_FOD_Drive> -LimitAccess
     ```

8. Al termine dell'indicatore di stato, riavviare il sistema operativo.

 Per altre informazioni sui comandi DISM, vedere [usare DISM in Windows PowerShell](https://docs.microsoft.com/windows-hardware/manufacture/desktop/use-dism-in-windows-powershell-s14)

## <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>Facoltativamente, aggiungere Internet Explorer 11 per Server Core (dopo l'aggiunta di Server Core App compatibilità DOM)

 >[!NOTE]  
   > Server Core App compatibilità DOM è obbligatorio per l'aggiunta di Internet Explorer 11, ma non è necessario aggiungere il DOM di compatibilità di Server Core App Internet Explorer 11.

1. Accedere come amministratore nel computer Server Core con il DOM di compatibilità delle App già aggiunto e il pacchetto facoltativo di DOM Server che ISO copiato in locale.

2. Avviare PowerShell immettendo **powershell.exe** un prompt dei comandi.

3. Montare l'ISO FOD usando il comando seguente:

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

4. Eseguire questo comando, usando il `$package_path` variabile in cui immettere il percorso del file cab di Internet Explorer:

    ```PowerShell
    $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

    Add-WindowsPackage -Online -PackagePath $package_path
    ```

5. Al termine dell'indicatore di stato, riavviare il sistema operativo.

## <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>Note sulla versione e i suggerimenti per il pacchetto facoltativo DOM di compatibilità delle App di Server Core e Internet Explorer 11

> [!IMPORTANT]
> FODs installato in Windows Server, versione 1809 non rimane invariati dopo un aggiornamento sul posto a Windows Server, versione 1903, pertanto è necessario installarli dopo l'aggiornamento. In alternativa, è possibile aggiungere FODs per la nuova origine di installazione di Windows Server prima dell'aggiornamento. Ciò garantisce che la nuova versione di qualsiasi FODs siano presenti dopo aver completato l'aggiornamento. Per altre informazioni, vedere la [aggiunta di funzionalità e pacchetti facoltativi in un'immagine di base del Server WIM offline](install-fod-19.md#add-capabilities).

- **Importante:** Leggere le note sulla versione di Windows Server 2019 per eventuali problemi, considerazioni o informazioni aggiuntive prima di procedere all'installazione e l'uso del pacchetto facoltativo DOM di compatibilità delle App di Server Core e Internet Explorer 11.

- È possibile riscontrare sfarfallio con l'uso della console Server Core, quando si aggiunge il DOM di compatibilità delle App dopo l'utilizzo di Windows Update per installare gli aggiornamenti cumulativi.  Aggiorna questo problema viene risolto con dicembre 2018.  Per altre informazioni e la risoluzione, vedere [4481610 articolo della Knowledge Base: Sfarfallio dopo l'installazione Server Core App compatibilità DOM in Server Core di Windows Server 2019](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core).

- Dopo l'installazione del DOM di compatibilità delle App e riavvio del server, verrà modificato il colore della cornice finestra console comando su una sfumatura di blu diversi.

- Se si sceglie di installare anche il pacchetto facoltativo di Internet Explorer 11, si noti che facendo doppio clic per aprire file con estensione htm salvato localmente non è supportato. Tuttavia, è possibile **fare doppio clic su** e scegliere **aprire con Internet Explorer**, o è possibile aprirlo direttamente da Internet Explorer **File** -> **aprire**.

- Per migliorare ulteriormente la compatibilità delle app di base del Server con il DOM di compatibilità delle App, la Console di gestione IIS è stato aggiunto a Server Core come componente facoltativo.  Tuttavia, è assolutamente necessario per prima cosa aggiungere DOM compatibilità delle App per usare la Console di gestione IIS. Console di gestione IIS si basa su Microsoft Management Console (mmc.exe), che è disponibile solo in Server Core con l'aggiunta di DOM compatibilità delle App.  Usare Powershell [ **Install-WindowsFeature** ](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) aggiungere Console di gestione IIS.

- Come un punto generale di indicazioni, quando installare le App in Server Core (con o senza questi pacchetti facoltativi), talvolta è necessario usare le istruzioni e le opzioni di installazione invisibile all'utente. 
    
 - Ad esempio, SQL Server Management Studio per SQL Server 2016 e SQL Server 2017 possono essere installato in Server Core e funziona perfettamente quando è presente il DOM di compatibilità delle App.  Vedere [installare SQL Server dal Prompt dei comandi](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017).
 - Se SQL Server Management Studio non è desiderato, quindi non è necessario installare Server Core App compatibilità DOM.  Vedere [installare SQL Server in Server Core](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017).

## <a name="a-idadd-capabilities-adding-capabilities-and-optional-packages-to-an-offline-wim-server-core-image"></a><a id="add-capabilities"> Aggiunta di funzionalità e pacchetti facoltativi in un'immagine di base del Server WIM offline

1. Scaricare i file di immagine ISO DOM Server e Windows Server in una cartella locale in un computer Windows.

   - Se si dispone di un contratto multilicenza è possibile scaricare i file di immagine Windows Server e Server DOM ISO dal [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
   - Il file di immagine ISO DOM Server è disponibile anche nella [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server) o scegliere il [portale di Visual Studio](https://visualstudio.microsoft.com) per i sottoscrittori.

2. Aprire una sessione di PowerShell come amministratore e quindi usare i comandi seguenti per montare i file di immagine come unità:

   ```PowerShell
   Mount-DiskImage -ImagePath Path_To_ServerFOD_ISO
   Mount-DiskImage -ImagePath Path_To_Windows_Server_ISO
   ```

3. Copia il contenuto del file ISO di Windows Server in una cartella locale (ad esempio, *C:\SetupFiles\WindowsServer*).

4. Ottiene il nome dell'immagine che si desidera modificare all'interno del file Install. wim con il comando seguente.<br>
Usare il `$install_wim_path` variabile in cui immettere il percorso al file Install. wim, che si trova all'interno della cartella \Sources del file ISO.

   ```PowerShell
   $install_wim_path = "C:\SetupFiles\WindowsServer\sources\install.wim"

   Get-WindowsImage -ImagePath $install_wim_path
   ```

5. Montare il file Install. wim in una nuova cartella con i seguenti comandi sostituendo i valori delle variabili di esempio con quelli personalizzati e riutilizza le `$install_wim_path` variabile dal comando precedente.<br>

   - `$image_name` -Immettere il nome dell'immagine che si desidera montare.
   - `$mount_folder variable` -Specificare la cartella da utilizzare per l'accesso al contenuto del file Install. wim.

   ```PowerShell
   $image_name = "Windows Server Datacenter"
   $mount_folder = "c:\test\offline"

   Mount-WindowsImage -ImagePath $install_wim_path -Name $image_name -path $mount_folder
   ```

6. Aggiungere le funzionalità e i pacchetti desiderati per l'immagine Install. wim montato usando i comandi seguenti, sostituendo i valori delle variabili di esempio con quelli personalizzati.<br>

   - `$capability_name` -Specificare il nome della funzionalità per installare (in questo caso, la funzionalità AppCompatibility).
   - `$package_path` -Specificare il percorso per il pacchetto di installazione (in questo caso, Internet Explorer).
   - `$fod_drive` -Specificare la lettera di unità dell'immagine Server DOM montata.

   ```PowerShell
   $capability_name = "ServerCore.AppCompatibility~~~~0.0.1.0"
   $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"
   $fod_drive = "d:\"

   Add-WindowsCapability -Path $mount_folder -Name $capability_name -Source $fod_drive -LimitAccess
   Add-WindowsPackage -Path $mount_folder -PackagePath $package_path
   ```

7. Smontare ed eseguire il commit delle modifiche al file Install. wim utilizzando il comando seguente, che usa il `$mount_folder` variabile dai comandi precedenti:

   ```PowerShell
   Dismount-WindowsImage -Path $mount_folder -Save
   ```

È ora possibile aggiornare il server eseguendo setup.exe dalla cartella creata per i file di installazione di Windows Server (in questo esempio: *C:\SetupFiles\WindowsServer*). Questa cartella contiene ora i file di installazione di Windows Server con le funzionalità aggiuntive e facoltative dei pacchetti inclusi.