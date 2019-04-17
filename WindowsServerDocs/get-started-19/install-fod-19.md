---
title: Funzionalità su richiesta di compatibilità dell'app Server Core
description: Come installare Windows Server funzionalità su richiesta
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
ms.sourcegitcommit: dbb4738fdac3b7911952ff11f1eaed9649d6567a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/07/2019
ms.locfileid: "9149901"
---
# Funzionalità su richiesta di compatibilità dell'app Server Core

> Si applica a Windows Server 2019 e Windows Server, versione 1809

La **Funzionalità di compatibilità delle App di Server Core su richiesta** è un pacchetto di funzionalità facoltativa che possa essere aggiunti per le installazioni Server Core di Windows Server 2019 o Windows Server, versione 1809, in qualsiasi momento.

Per altre informazioni sulle funzionalità su richiesta (FOD), vedi [Funzionalità su richiesta](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).


## Il motivo per cui installare l'App compatibilità? 

Compatibilità delle App, una funzionalità su richiesta per Server Core, migliora notevolmente la compatibilità di app dell'opzione di installazione di Windows Server Core, includendo un sottoinsieme di file binari e i pacchetti di Windows Server con esperienza Desktop, senza dover aggiungere le Ambiente grafico di esperienza Desktop di Windows Server. Questo pacchetto facoltativo è disponibile in un file ISO separato o da Windows Update, ma possa essere aggiunti solo a immagini e le installazioni di Windows Server Core.

I due valori primari che fornisce FOD la compatibilità delle App sono:

1.  Aumenta la compatibilità di Server Core per le applicazioni server che sono già nel mercato o sono già state sviluppata da alle organizzazioni e distribuito.

2.  Collabora con fornendo componenti del sistema operativo e compatibilità maggiore di strumenti software usati in latino, risoluzione dei problemi e scenari di debug.

Componenti del sistema operativo che sono disponibili come parte dei Server Core App compatibilità DOM includono:

-   Microsoft Management Console (mmc.exe)

-   Visualizzatore eventi (Eventvwr. msc)

-   Performance Monitor (PerfMon.exe)

-   Monitoraggio delle risorse (Resmon.exe)

-   Gestione di dispositivi (devmgmt. msc)

-   Esplora file (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   Gestione disco (diskmgmt. msc)

-   Gestione Cluster di failover (Cluadmin)

    -   Richiede l'aggiunta della funzionalità di Windows Server di Clustering di Failover prima di tutto.

        -   Utilizzare i Cmdlet di Powershell per aggiungere, `Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools`

        -   Per eseguire Gestione Cluster di Failover, Immetti **cluadmin** al prompt dei comandi.

## L'installazione la compatibilità delle App FOD

 >[!IMPORTANT] 
   >FOD la compatibilità delle App può essere installato solo su Server Core. Non tentare di aggiungere il FOD compatibilità delle App di Server Core a un'installazione di Windows Server di Windows Server con esperienza Desktop.

### Per aggiungere la funzionalità di compatibilità dell'App Server Core su richiesta a un'istanza di esecuzione di Server Core

 >[!NOTE] 
   > Questa procedura Usa Deployment Image Servicing and Management (DISM.exe), uno strumento da riga di comando. Per ulteriori informazioni sui comandi di manutenzione, vedi [DISM opzioni della riga di comando di manutenzione pacchetti di funzionalità](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-capabilities-package-servicing-command-line-options).

>[!NOTE] 
   > I pacchetti facoltativi FOD stesso ISO possono essere usati per le installazioni Server Core di Windows Server 2019, o Windows Server, versione 1809, le installazioni.

>[!NOTE] 
   > Se il computer o macchina virtuale che esegue Server Core è in grado di connettersi a Windows Update, è possibile saltare i passaggi 1-7 di seguito. Ma assicurati di non attivare /origine e /LimitAccess dal comando DISM nel passaggio 8.

1. Scaricare i pacchetti facoltativi FOD Server ISO e copiare il file ISO in una cartella condivisa nella rete locale:

 - Se hai un contratto multilicenza, è possibile scaricare il file di immagine ISO di FOD Server dal portale in cui viene ottenuto il file di immagine ISO del sistema operativo stesso: [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
 - Il file di immagine ISO di FOD Server è anche disponibile in [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) o nel [portale di Visual Studio](https://visualstudio.microsoft.com) per gli abbonati.


2. Accedi come amministratore nel computer che è connesso alla rete locale e che si desidera aggiungere il FOD a Server Core.

3. Utilizzare **net use**o un altro metodo per connettersi alla posizione di FOD ISO.

4. Copia il ISO FOD in una cartella locale di tua scelta.

5. Avvia PowerShell immettendo **powershell.exe** in un prompt dei comandi.

6. Montare il ISO FoD utilizzando il comando seguente:

        Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

7. Digitare **exit** per uscire da PowerShell.

8.  Eseguire il comando seguente:

        DISM /Online /Add-Capability /CapabilityName:"ServerCore.AppCompatibility~~~~0.0.1.0" /Source:drive_letter_of_mounted_ISO: /LimitAccess

9.  Al termine dell'indicatore, riavvia il sistema operativo.

### Facoltativamente, aggiungere Internet Explorer 11 a Server Core (dopo l'aggiunta di Server Core App compatibilità DOM)

 >[!NOTE]  
   > Compatibilità DOM App Server Core è necessario per l'aggiunta di Internet Explorer 11, ma Internet Explorer 11 non è necessario aggiungere la FOD compatibilità delle App di Server Core.

1.  Accedi come amministratore sul computer Server Core che ha già aggiunto il FOD di compatibilità delle App e il pacchetto facoltativo FOD Server che ISO copiato in locale.

2.  Avvia PowerShell immettendo **powershell.exe** in un prompt dei comandi.

3.  Montare il ISO FoD utilizzando il comando seguente:

         Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

4.  Digitare **exit** per uscire da PowerShell.


5.  Eseguire il comando seguente:

        Dism /online /add-package:drive_letter_of_mounted_iso:"Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

6.  Al termine dell'indicatore, riavvia il sistema operativo.

 
#### Note sulla versione e i suggerimenti per il pacchetto facoltativo FOD di compatibilità delle App di Server Core e Internet Explorer 11

- **Importante:** leggere le note sulla versione di Windows Server 2019 per eventuali problemi, considerazioni o linee guida prima di procedere con l'installazione e l'utilizzo del pacchetto facoltativo FOD di compatibilità delle App di Server Core e Internet Explorer 11.
 
 >[!NOTE] 
   > È possibile che si verifichino sfarfallio con l'esperienza di console di Server Core durante l'aggiunta di FOD la compatibilità delle App dopo utilizzando Windows Update per l'installazione di aggiornamenti cumulativi.  Aggiorna il problema viene risolto con dicembre 2018.  Per altre informazioni e risoluzione, vedere [articolo della Knowledge Base 4481610: sfarfallio dopo l'installazione di FOD di compatibilità delle App di Server Core in Windows Server 2019 Server Core](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core).

- Dopo l'installazione delle App compatibilità FOD e riavvio del server, il colore di frame della finestra di comando console verrà modificato in una tonalità diversa di blu.

- Se si sceglie di installare anche il pacchetto facoltativo di Internet Explorer 11, tieni presente che facendo doppio clic per aprire i file salvati localmente htm non è supportato. Tuttavia, è possibile **pulsante destro del mouse** e scegli **Apri con Internet Explorer**oppure può aprirlo direttamente da Internet Explorer **File** -> **Apri**. 

- Per migliorare ulteriormente la compatibilità delle app di Server Core con FOD la compatibilità delle App, la Console di gestione di IIS è stato aggiunto a Server Core come un componente facoltativo.  Tuttavia, è assolutamente necessario innanzitutto aggiungere il FOD di compatibilità delle App per usare la Console di gestione IIS. Console di gestione IIS si basa su Microsoft Management Console (mmc.exe), che è disponibile solo in Server Core con l'aggiunta di FOD la compatibilità delle App.  Usare Powershell [**Install-WindowsFeature**](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) per aggiungere la Console di gestione IIS.

- Come un punto generale di linee guida, quando l'installazione di App in Server Core (con o senza questi pacchetti facoltativi) viene a volte è necessario utilizzare le istruzioni e le opzioni di installazione invisibile all'utente. 
    
 - Ad esempio, SQL Server Management Studio per SQL Server 2016 e 2017 di SQL Server può essere installato in Server Core e sia completamente funzionante quando FOD la compatibilità delle App è presente.  Vedi, [installare SQL Server dal Prompt dei comandi](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017).
 - Se non si vuole SQL Server Management Studio, non è necessario installare il Server Core App compatibilità.  Vedi, [installare SQL Server in Server Core](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017).

