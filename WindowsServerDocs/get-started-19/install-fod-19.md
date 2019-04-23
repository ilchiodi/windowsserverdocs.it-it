---
title: Funzionalità su richiesta di compatibilità dell'app Server Core
description: Come installare funzionalità di Windows Server su richiesta
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: b8211ace56aa6565295a15adce26a8dfbc98e1e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866112"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>Funzionalità su richiesta di compatibilità dell'app Server Core

> Si applica a Windows Server 2019 e Windows Server, versione 1809

Il **Server Core App compatibilità funzionalità su richiesta** è un pacchetto di funzionalità facoltativa che può essere aggiunto per le installazioni Server Core di Windows Server 2019 o Windows Server, versione 1809, in qualsiasi momento.

Per altre informazioni sulle funzionalità su richiesta (DOM), vedere [funzionalità su richiesta](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).


## <a name="why-install-the-app-compatibility-fod"></a>Il motivo per cui installare il DOM di compatibilità delle App? 

Compatibilità dell'applicazione, una funzionalità su richiesta per Server Core, migliora notevolmente la compatibilità dell'applicazione dell'opzione di installazione di Windows Server Core, includendo un subset di file binari e i pacchetti da Windows Server con esperienza Desktop, senza l'aggiunta di Ambiente con interfaccia grafica di esperienza Desktop di Windows Server. Questo pacchetto facoltativo è disponibile su un'immagine ISO separata o da Windows Update, ma può essere aggiunti solo alle immagini e le installazioni di Windows Server Core.

I due valori primari che fornisce il DOM di compatibilità delle App sono:

1.  Aumenta la compatibilità di Server Core per le applicazioni server che sono già nel mercato o già sviluppato da organizzazioni e distribuito.

2.  Collabora con la fornitura di componenti del sistema operativo e una maggiore compatibilità delle applicazioni software dell'uso di strumenti acuta risoluzione dei problemi e gli scenari di debug.

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

        -   Usare i Cmdlet di Powershell per aggiungere, `Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools`

        -   Per eseguire Gestione Cluster di Failover, immettere **cluadmin** al prompt dei comandi.

## <a name="installing-the-app-compatibility-fod"></a>Installare la compatibilità delle applicazioni DOM

 >[!IMPORTANT] 
   >Il DOM di compatibilità delle App può essere installato solo in Server Core. Non tentare di aggiungere Server Core App compatibilità DOM per un'installazione di Windows Server di Windows Server con esperienza Desktop.

### <a name="to-add-the-server-core-app-compatibility-feature-on-demand-fod-to-a-running-instance-of-server-core"></a>Per aggiungere la funzionalità di compatibilità delle applicazioni Server Core su richiesta (DOM) a un'istanza in esecuzione di Server Core

 >[!NOTE] 
   > Questa procedura Usa Deployment Image Servicing and Management (DISM.exe), uno strumento da riga di comando. Per altre informazioni sui comandi DISM, vedere [DISM le funzionalità del pacchetto di manutenzione Command-Line Options](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-capabilities-package-servicing-command-line-options).

>[!NOTE] 
   > Gli stessi pacchetti facoltativi DOM ISO sono utilizzabile per le installazioni Server Core di Windows Server 2019, o Windows Server, versione 1809, le installazioni.

>[!NOTE] 
   > Se il computer o macchina virtuale che esegue Server Core è in grado di connettersi a Windows Update, è possibile saltare i passaggi da 1 a 7 di sotto. Tuttavia, assicurarsi di lasciare disattivato /Source e /LimitAccess dal comando DISM nel passaggio 8.

1. Scaricare i pacchetti facoltativi Server DOM ISO e copiare l'immagine ISO a una cartella condivisa nella rete locale:

 - Se si dispone di un contratto multilicenza è possibile scaricare il file di immagine ISO DOM Server dallo stesso portale in cui viene ottenuto il file di immagine ISO del sistema operativo: [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
 - Il file di immagine ISO DOM Server è disponibile anche nella [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) o scegliere il [portale di Visual Studio](https://visualstudio.microsoft.com) per i sottoscrittori.


2. Accedere come amministratore nel computer Server Core che è connesso alla rete locale e che si desidera aggiungere al DOM.

3. Usare **net usare**, o un altro metodo, per connettersi al percorso del FOD ISO.

4. Copiare FOD ISO in una cartella locale di propria scelta.

5. Avviare PowerShell immettendo **powershell.exe** un prompt dei comandi.

6. Montare l'ISO FoD usando il comando seguente:

        Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

7. Tipo di **uscire** per uscire da PowerShell.

8.  Eseguire il comando seguente:

        DISM /Online /Add-Capability /CapabilityName:"ServerCore.AppCompatibility~~~~0.0.1.0" /Source:drive_letter_of_mounted_ISO: /LimitAccess

9.  Al termine dell'indicatore di stato, riavviare il sistema operativo.

### <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>Facoltativamente, aggiungere Internet Explorer 11 per Server Core (dopo l'aggiunta di Server Core App compatibilità DOM)

 >[!NOTE]  
   > Server Core App compatibilità DOM è obbligatorio per l'aggiunta di Internet Explorer 11, ma non è necessario aggiungere il DOM di compatibilità di Server Core App Internet Explorer 11.

1.  Accedere come amministratore nel computer Server Core con il DOM di compatibilità delle App già aggiunto e il pacchetto facoltativo di DOM Server che ISO copiato in locale.

2.  Avviare PowerShell immettendo **powershell.exe** un prompt dei comandi.

3.  Montare l'ISO FoD usando il comando seguente:

         Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

4.  Tipo di **uscire** per uscire da PowerShell.


5.  Eseguire il comando seguente:

        Dism /online /add-package:drive_letter_of_mounted_iso:"Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

6.  Al termine dell'indicatore di stato, riavviare il sistema operativo.

 
#### <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>Note sulla versione e i suggerimenti per il pacchetto facoltativo DOM di compatibilità delle App di Server Core e Internet Explorer 11

- **Importante:** , leggere le note sulla versione di Windows Server 2019 per eventuali problemi, considerazioni o informazioni aggiuntive prima di procedere all'installazione e l'uso del pacchetto facoltativo DOM di compatibilità delle App di Server Core e Internet Explorer 11.
 
 >[!NOTE] 
   > È possibile riscontrare sfarfallio con l'uso della console Server Core, quando si aggiunge il DOM di compatibilità delle App dopo l'utilizzo di Windows Update per installare gli aggiornamenti cumulativi.  Aggiorna questo problema viene risolto con dicembre 2018.  Per altre informazioni e la risoluzione, vedere [4481610 articolo della Knowledge Base: Sfarfallio dopo l'installazione Server Core App compatibilità DOM in Server Core di Windows Server 2019](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core).

- Dopo l'installazione del DOM di compatibilità delle App e riavvio del server, verrà modificato il colore della cornice finestra console comando su una sfumatura di blu diversi.

- Se si sceglie di installare anche il pacchetto facoltativo di Internet Explorer 11, si noti che facendo doppio clic per aprire file con estensione htm salvato localmente non è supportato. Tuttavia, è possibile **fare doppio clic su** e scegliere **aprire con Internet Explorer**, o è possibile aprirlo direttamente da Internet Explorer **File** -> **aprire**. 

- Per migliorare ulteriormente la compatibilità delle app di base del Server con il DOM di compatibilità delle App, la Console di gestione IIS è stato aggiunto a Server Core come componente facoltativo.  Tuttavia, è assolutamente necessario per prima cosa aggiungere DOM compatibilità delle App per usare la Console di gestione IIS. Console di gestione IIS si basa su Microsoft Management Console (mmc.exe), che è disponibile solo in Server Core con l'aggiunta di DOM compatibilità delle App.  Usare Powershell [ **Install-WindowsFeature** ](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) aggiungere Console di gestione IIS.

- Come un punto generale di indicazioni, quando installare le App in Server Core (con o senza questi pacchetti facoltativi), talvolta è necessario usare le istruzioni e le opzioni di installazione invisibile all'utente. 
    
 - Ad esempio, SQL Server Management Studio per SQL Server 2016 e SQL Server 2017 possono essere installato in Server Core e funziona perfettamente quando è presente il DOM di compatibilità delle App.  Vedere [installare SQL Server dal Prompt dei comandi](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017).
 - Se SQL Server Management Studio non è desiderato, quindi non è necessario installare Server Core App compatibilità DOM.  Vedere [installare SQL Server in Server Core](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017).

