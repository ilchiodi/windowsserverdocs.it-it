---
title: Funzionalità su richiesta di compatibilità app Server Core
description: Come installare funzionalità su richiesta di Windows Server
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 9ba0303d1eebc2138db7c5f1428ce920b0cb8af0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360816"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>Funzionalità su richiesta di compatibilità app Server Core

> Si applica a: Windows Server 2019, Canale semestrale di Windows Server

La **funzionalità su richiesta di compatibilità app Server Core** è un pacchetto di funzionalità facoltativo che può essere aggiunto in qualsiasi momento alle installazioni Server Core di Windows Server 2019 o quelle del Canale semestrale di Windows Server.

Per altre informazioni sulle funzionalità su richiesta, vedi [Funzionalità su richiesta](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).

## <a name="why-install-the-app-compatibility-fod"></a>Perché installare la funzionalità su richiesta di compatibilità app?

Compatibilità app, una funzionalità su richiesta per Server Core, migliora notevolmente la compatibilità delle app dell'opzione di installazione Windows Server Core attraverso l'inclusione di un sottoinsieme di file binari e pacchetti di Windows Server con Esperienza desktop, senza l'aggiunta dell'ambiente grafico Esperienza desktop di Windows Server. Questo pacchetto facoltativo è disponibile in un file ISO separato o da Windows Update e può essere aggiunto solo alle immagini e alle installazioni Server Core di Windows.

La funzionalità su richiesta di compatibilità app offre due vantaggi principali:

- Aumenta la compatibilità di Server Core per le applicazioni server che sono già sul mercato o che sono state già sviluppate da organizzazioni e distribuite.
- Fornisce supporto mettendo a disposizione componenti del sistema operativo e una maggiore compatibilità delle app degli strumenti software usati in scenari di debug e risoluzione dei problemi critici.

I componenti del sistema operativo che sono disponibili come parte della funzionalità su richiesta di compatibilità app Server Core includono:

-   Microsoft Management Console (mmc.exe)

-   Visualizzatore eventi (Eventvwr.msc)

-   Performance Monitor (PerfMon.exe)

-   Monitoraggio risorse (Resmon.exe)

-   Gestione dispositivi (Devmgmt.msc)

-   Esplora file (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   Gestione disco (Diskmgmt.msc)

-   Gestione cluster di failover (CluAdmin.msc)

    -   Richiede prima l'aggiunta della funzionalità Clustering di failover di Windows Server.

        -   Da una sessione di PowerShell con privilegi elevati: 

            ```PowerShell
            Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools
            ```

        -   Per eseguire Gestione cluster di failover, al prompt dei comandi immetti **cluadmin**.

I server che eseguono Windows Server versione 1903 e successive inoltre supportano i componenti seguenti (se viene usata la stessa versione della funzionalità su richiesta di compatibilità app):

- Console di gestione di Hyper-V (virtmgmt.msc)
- Utilità di pianificazione (taskschd.msc)

## <a name="installing-the-app-compatibility-fod"></a>Installazione della funzionalità su richiesta di compatibilità app

La funzionalità su richiesta di compatibilità app può essere installata solo in Server Core. Non tentare di aggiungere la funzionalità su richiesta di compatibilità app Server Core a un'installazione di Windows Server con Esperienza desktop. È possibile usare lo stesso file ISO dei pacchetti facoltativi di funzionalità su richiesta per le installazioni Server Core di Windows Server 2019 o per le installazioni del Canale semestrale di Windows Server.

1. Se il server può connettersi a Windows Update, è sufficiente eseguire il comando seguente da una sessione di PowerShell con privilegi elevati e quindi riavviare Windows Server dopo l'esecuzione del comando:

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0
    ```

2. Se il server non può connettersi a Windows Update, scarica il file ISO dei pacchetti facoltativi di funzionalità su richiesta del server e copialo in una cartella condivisa nella rete locale:

   - Se disponi di un contratto multilicenza, puoi scaricare il file di immagine ISO della funzionalità su richiesta del server dallo stesso portale da cui reperisci il file di immagine ISO del sistema operativo: [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
   - Il file di immagine ISO della funzionalità su richiesta del server è disponibile anche in [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server) o nel [portale di Visual Studio](https://visualstudio.microsoft.com) per i sottoscrittori.

3. Accedi con un account amministratore nel computer Server Core che è connesso alla rete locale e a cui vuoi aggiungere la funzionalità su richiesta.

4. Usa **net use** o un altro metodo per connetterti al percorso del file ISO della funzionalità su richiesta.

5. Copia il file ISO della funzionalità su richiesta in una cartella locale di tua scelta.

6. Monta il file ISO della funzionalità su richiesta eseguendo il comando seguente in una sessione di PowerShell con privilegi elevati:

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

7. Eseguire il comando seguente:

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0 -Source <Mounted_Server_FOD_Drive> -LimitAccess
     ```

8. Al termine dell'avanzamento dell'indicatore di stato, riavvia il sistema operativo.

   Per altre informazioni sui comandi di Gestione e manutenzione immagini distribuzione, vedi [Usare Gestione e manutenzione immagini distribuzione in Windows PowerShell](https://docs.microsoft.com/windows-hardware/manufacture/desktop/use-dism-in-windows-powershell-s14).

## <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>Per aggiungere facoltativamente Internet Explorer 11 a Server Core (dopo l'aggiunta della funzionalità su richiesta di compatibilità app Server Core)

 >[!NOTE]  
   > La funzionalità su richiesta di compatibilità app Server Core è necessaria per l'aggiunta di Internet Explorer 11, ma non viceversa.

1. Accedi come amministratore nel computer Server Core in cui è già stata aggiunta la funzionalità su richiesta di compatibilità app Server Core e in cui è stato copiato in locale il file ISO dei pacchetti facoltativi della funzionalità su richiesta del server.

2. Avvia PowerShell immettendo **powershell.exe** al prompt dei comandi.

3. Monta il file ISO della funzionalità su richiesta usando il comando riportato di seguito:

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

4. Esegui questo comando usando la variabile `$package_path` per immettere il percorso del file con estensione cab di Internet Explorer:

    ```PowerShell
    $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

    Add-WindowsPackage -Online -PackagePath $package_path
    ```

5. Al termine dell'avanzamento dell'indicatore di stato, riavvia il sistema operativo.

## <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>Note sulla versione e suggerimenti per il pacchetto facoltativo di Internet Explorer 11 e la funzionalità su richiesta di compatibilità app Server Core

> [!IMPORTANT]
> Le funzionalità su richiesta installate in Windows Server versione 1809 non verranno mantenute dopo un aggiornamento sul posto a Windows Server versione 1903 e pertanto dovranno essere reinstallate dopo l'aggiornamento. In alternativa, puoi aggiungere le funzionalità su richiesta nell'origine della nuova installazione di Windows Server prima dell'aggiornamento. In questo modo la nuova versione di eventuali funzionalità su richiesta sarà presente al termine dell'aggiornamento. Per altre informazioni, vedi [Aggiunta di funzionalità e pacchetti facoltativi a un'immagine WIM Server Core offline](install-fod-19.md#add-capabilities).

- **Importante:** leggi le note sulla versione di Windows Server 2019 per eventuali problemi, considerazioni o informazioni aggiuntive prima di procedere all'installazione e all'uso del pacchetto facoltativo di Internet Explorer 11 e della funzionalità su richiesta di compatibilità app Server Core.

- È possibile riscontrare uno sfarfallio nella console Server Core mentre aggiungi la funzionalità su richiesta di compatibilità app dopo l'uso di Windows Update per installare aggiornamenti cumulativi.  Questo problema viene risolto con gli aggiornamenti di dicembre 2018.  Per altre informazioni e i passaggi per la risoluzione dei problemi, vedi l'[articolo 4481610 della Knowledge Base: Sfarfallio dopo l'installazione della funzionalità su richiesta di compatibilità app Server Core in Windows Server 2019 Server Core](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core).

- Dopo l'installazione della funzionalità su richiesta di compatibilità app e il riavvio del server, il colore della cornice della finestra della console dei comandi assumerà una diversa sfumatura di blu.

- Se scegli di installare anche il pacchetto facoltativo di Internet Explorer 11, considera che non è supportata l'apertura tramite doppio clic dei file con estensione htm salvati localmente. Puoi tuttavia **fare clic con il pulsante destro del mouse** e scegliere **Apri con Internet Explorer** oppure puoi procedere direttamente da Internet Explorer facendo clic su **File** -> **Apri**.

- Per migliorare ulteriormente la compatibilità app di Server Core con la funzionalità su richiesta di compatibilità app, a Server Core è stato aggiunto il componente facoltativo Console di gestione IIS.  Per usare Console di gestione IIS è tuttavia assolutamente necessario aggiungere prima la funzionalità su richiesta di compatibilità app. Console di gestione IIS si basa su Microsoft Management Console (mmc.exe), che è disponibile solo in Server Core con l'aggiunta della funzionalità su richiesta di compatibilità app.  Per aggiungere Console di gestione IIS, usa [**Install-WindowsFeature**](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) di PowerShell.

- Come indicazione generale, quando installi le app in Server Core (con o senza questi pacchetti facoltativi), talvolta è necessario usare le istruzioni e le opzioni di installazione invisibile all'utente. 
    
  - È ad esempio possibile installare SQL Server Management Studio per SQL Server 2016 e SQL Server 2017 in Server Core e il funzionamento è garantito se è presente la funzionalità su richiesta di compatibilità app.  Vedi [Installare SQL Server dal prompt dei comandi](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017).
  - Se non vuoi usare SQL Server Management Studio, non è necessario installare la funzionalità su richiesta di compatibilità app Server Core.  Vedi [Installare SQL Server in Server Core](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017).

## <a name="a-idadd-capabilities-adding-capabilities-and-optional-packages-to-an-offline-wim-server-core-image"></a><a id="add-capabilities">Aggiunta di funzionalità e pacchetti facoltativi a un'immagine WIM Server Core offline

1. Scarica i file di immagine ISO di Windows Server e della funzionalità su richiesta del server in una cartella locale in un computer Windows.

   - Se disponi di un contratto multilicenza, puoi scaricare i file di immagine ISO di Windows Server e della funzionalità su richiesta del server da [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
   - Il file di immagine ISO della funzionalità su richiesta del server è disponibile anche per le versioni di Long-Term Servicing Channel in [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server) o nel [portale di Visual Studio](https://visualstudio.microsoft.com) per i sottoscrittori.

2. Apri una sessione di PowerShell come amministratore, quindi usa i comandi seguenti per montare i file di immagine come unità:

   ```PowerShell
   Mount-DiskImage -ImagePath Path_To_ServerFOD_ISO
   Mount-DiskImage -ImagePath Path_To_Windows_Server_ISO
   ```

3. Copia il contenuto del file ISO di Windows Server in una cartella locale, ad esempio, *C:\SetupFiles\WindowsServer*.

4. Ottieni il nome dell'immagine che vuoi modificare all'interno del file Install.wim con il comando seguente.<br>
Usa la variabile `$install_wim_path` per immettere il percorso del file Install.wim, che si trova all'interno della cartella \Sources del file ISO.

   ```PowerShell
   $install_wim_path = "C:\SetupFiles\WindowsServer\sources\install.wim"

   Get-WindowsImage -ImagePath $install_wim_path
   ```

5. Monta il file Install.wim in una nuova cartella con il comando seguente sostituendo i valori delle variabili di esempio con valori personalizzati e riusando la variabile `$install_wim_path` del comando precedente.<br>

   - `$image_name` - Immetti il nome dell'immagine che vuoi montare.
   - `$mount_folder variable` -Specifica la cartella da usare per l'accesso al contenuto del file Install.wim.

   ```PowerShell
   $image_name = "Windows Server Datacenter"
   $mount_folder = "c:\test\offline"

   Mount-WindowsImage -ImagePath $install_wim_path -Name $image_name -path $mount_folder
   ```

6. Aggiungi le funzionalità e i pacchetti desiderati all'immagine Install.wim montata tramite i comandi seguenti, sostituendo i valori delle variabili di esempio con valori personalizzati.<br>

   - `$capability_name` -Specifica il nome della funzionalità da installare, in questo caso la funzionalità AppCompatibility.
   - `$package_path` -Specifica il percorso del pacchetto da installare, in questo caso Internet Explorer.
   - `$fod_drive` -Specifica la lettera dell'unità dell'immagine della funzionalità su richiesta del server montata.

   ```PowerShell
   $capability_name = "ServerCore.AppCompatibility~~~~0.0.1.0"
   $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"
   $fod_drive = "d:\"

   Add-WindowsCapability -Path $mount_folder -Name $capability_name -Source $fod_drive -LimitAccess
   Add-WindowsPackage -Path $mount_folder -PackagePath $package_path
   ```

7. Procedi con lo smontaggio, quindi esegui il commit delle modifiche del file Install.wim usando il comando seguente, che usa la variabile `$mount_folder` dai comandi precedenti:

   ```PowerShell
   Dismount-WindowsImage -Path $mount_folder -Save
   ```

Puoi ora aggiornare il server eseguendo setup.exe dalla cartella creata per i file di installazione di Windows Server, in questo esempio: *C:\SetupFiles\WindowsServer*. Questa cartella contiene ora i file di installazione di Windows Server con le funzionalità aggiuntive e i pacchetti facoltativi inclusi.