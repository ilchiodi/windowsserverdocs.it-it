---
title: Gestire Nano Server
description: aggiornamenti, manutenzione pacchetti, tracciabilità rete, monitoraggio prestazioni
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 599d6438-a506-4d57-a0ea-1eb7ec19f46e
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 8973302fc8a0c6bdb5b19f9296e711dcc6465589
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826802"
---
# <a name="manage-nano-server"></a>Gestire Nano Server

>Si applica a: Windows Server 2016

> [!IMPORTANT]
> A partire da Windows Server, versione 1709, Nano Server sarà disponibile solo come [immagine del sistema operativo di base del contenitore](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Per scoprire ciò cosa implica, vedi [Modifiche a Nano Server](nano-in-semi-annual-channel.md).   

Nano Server viene gestito in remoto. Non è presente alcuna funzionalità di accesso locale e non supporta Servizi terminal. Sono tuttavia disponibili numerose opzioni per la gestione di Nano Server in remoto, tra cui Windows PowerShell, Strumentazione gestione Windows (WMI), Gestione remota Windows e Servizi di gestione emergenze (EMS).  

Per usare uno strumento di gestione remota è probabile che sia necessario conoscere l'indirizzo IP di Nano Server. Sono disponibili vari modi per trovare l'indirizzo IP:  
  
-   Usare la Console di ripristino di emergenza di Nano Server (per informazioni dettagliate, vedere la sezione Uso della Console di ripristino di emergenza di Nano Server in questo argomento).  
  
-   Collegare un cavo seriale al computer e usare EMS.  
  
-   Usando il nome computer assegnato a Nano Server durante la configurazione, è possibile ottenere l'indirizzo IP con il comando ping. Ad esempio `ping NanoServer-PC /4`.  
  
## <a name="using-windows-powershell-remoting"></a>Uso della comunicazione remota di Windows PowerShell  
Per gestire Nano Server con la comunicazione remota di Windows PowerShell, è necessario aggiungere l'indirizzo IP di Nano Server all'elenco dei computer di gestione degli host attendibili, aggiungere l'account in uso agli amministratori di Nano Server e abilitare CredSSP, se si prevede di usare tale funzionalità.  

 >[!NOTE]  
    > Se la destinazione Nano Server e computer di gestione sono nella stessa foresta Active Directory Domain Services (o in foreste con una relazione di trust), non è necessario aggiungere Nano Server all'elenco di host attendibili, è possibile connettersi a Nano Server usando il nome di dominio completo Per esempio: PS C:\> Con Enter-PSSession - ComputerName nanoserver.contoso.com-Credential (Get-Credential)
  
  
Per aggiungere Nano Server all'elenco di host attendibili, eseguire questo comando a un prompt di Windows PowerShell con privilegi elevati:  
  
`Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
  
Per avviare la sessione remota di Windows PowerShell, avviare una sessione di Windows PowerShell locale con privilegi elevati e quindi eseguire questi comandi:  
  
  
```  
$ip = "\<IP address of Nano Server>"  
$user = "$ip\Administrator"  
Enter-PSSession -ComputerName $ip -Credential $user  
```  
  
  
È ora possibile eseguire i comandi di Windows PowerShell in Nano Server come di consueto.  
  
> [!NOTE]  
> Non tutti i comandi di Windows PowerShell sono disponibili in questa versione di Nano Server. Per vedere quali sono disponibili, eseguire `Get-Command -CommandType Cmdlet`  
  
Arrestare la sessione remota con il comando `Exit-PSSession`  
  
## <a name="using-windows-powershell-cim-sessions-over-winrm"></a>Uso di sessioni di Windows PowerShell CIM su WinRM  
È possibile usare sessioni CIM e istanze di Windows PowerShell per eseguire i comandi WMI tramite Gestione remota Windows (WinRM).  
  
Avviare la sessione CIM eseguendo questi comandi in un prompt di Windows PowerShell:  
  
  
```  
$ip = "<IP address of the Nano Server\>"  
$ip\Administrator  
$cim = New-CimSession -Credential $user -ComputerName $ip  
```  
  
  
Con la sessione stabilita, è possibile eseguire vari comandi WMI, ad esempio:  
  
  
```  
Get-CimInstance -CimSession $cim -ClassName Win32_ComputerSystem | Format-List *  
Get-CimInstance -CimSession $Cim -Query "SELECT * from Win32_Process WHERE name LIKE 'p%'"  
```  
  
  
## <a name="windows-remote-management"></a>Gestione remota Windows  
Gestione remota Windows (WinRM) consente di eseguire programmi in Nano Server in modalità remota. Per usare WinRM, configurare prima il servizio e quindi impostare la tabella codici con questi comandi a un prompt dei comandi con privilegi elevati:  
  
**WinRM quickconfig**  
  
**winrm set winrm/config/client @{TrustedHosts="<ip address of Nano Server"}**  
  
**chcp 65001**  
  
È ora possibile eseguire comandi in Nano Server in modalità remota. Ad esempio:  
  
**winrs-r\<indirizzo IP di Nano Server > - u: amministratore-p:\<password di amministratore di Nano Server > ipconfig**  
  
Per altre informazioni su Gestione remota Windows, vedere [Panoramica di Gestione remota Windows (WinRM)](https://technet.microsoft.com/library/dn265971.aspx).  
   
   
  
## <a name="running-a-network-trace-on-nano-server"></a>Esecuzione di una traccia di rete in Nano Server  
 La traccia Netsh, Tracelog.exe e Logman.exe non sono disponibili in Nano Server. Per acquisire pacchetti di rete, è possibile usare i cmdlet di Windows PowerShell seguenti:  
   
   
```  
New-NetEventSession [-Name]  
Add-NetEventPacketCaptureProvider -SessionName  
Start-NetEventSession [-Name]  
Stop-NetEventSession [-Name]  
```  
Questi cmdlet sono documentati in dettaglio nell'argomento [Network Event Packet Capture Cmdlets in Windows PowerShell](https://technet.microsoft.com/library/dn268520(v=wps.630).aspx) (Cmdlet di acquisizione di pacchetti di eventi di rete in Windows PowerShell).  

##<a name="installing-servicing-packages"></a>Installazione di pacchetti di manutenzione  
Se si vuole installare un pacchetto di manutenzione, usare il parametro -ServicingPackagePath (è possibile passare una matrice di percorsi ai file con estensione cab):  
  
`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -ServicingPackagePath \\path\to\kb123456.cab`  
  
Un hotfix o un pacchetto di manutenzione viene spesso scaricato come elemento KB che include un file CAB. Seguire questa procedura per estrarre il file con estensione cab, che è possibile installare con il parametro -ServicingPackagePath:  
  
1.  Scaricare il pacchetto di manutenzione dall'articolo della Knowledge Base associato o da [Microsoft Update Catalog](https://catalog.update.microsoft.com/v7/site/home.aspx). Salvarlo in una condivisione di rete o di directory locale, ad esempio: C:\ServicingPackages  
2.  Creare una cartella in cui verrà salvato il pacchetto di manutenzione estratto.  Esempio: c:\KB3157663_expanded  
3.  Aprire una console di Windows PowerShell e usare il comando `Expand` specificando il percorso al file del pacchetto di manutenzione con estensione msu, inclusi il parametro `-f:*` e il percorso in cui si vuole estrarre il pacchetto di manutenzione.  Per esempio:  `Expand "C:\ServicingPackages\Windows10.0-KB3157663-x64.msu" -f:* "C:\KB3157663_expanded"`  
  
    I file espansi devono avere un aspetto simile al seguente:  
C:>dir C:\KB3157663_expanded   
Volume in drive C is OS  
Volume Serial Number is B05B-CC3D  
   
      Directory di C:\KB3157663_expanded  
   
      04/19/2016  01:17 PM    \<DIR>          .  
      04/19/2016  01:17 PM    \<DIR>          ..  
        04/17/2016  12:31 AM               517 Windows10.0-KB3157663-x64-pkgProperties.txt  
04/17/2016  12:30 AM        93,886,347 Windows10.0-KB3157663-x64.cab  
04/17/2016  12:31 AM               454 Windows10.0-KB3157663-x64.xml  
04/17/2016  12:36 AM           185,818 WSUSSCAN.cab  
               4 File(s)     94,073,136 bytes  
               2 Dir(s)  328,559,427,584 bytes free  
4.  Eseguire `New-NanoServerImage` con il parametro - ServicingPackagePath che punta al file con estensione cab in questa directory, ad esempio: `New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -ServicingPackagePath C:\KB3157663_expanded\Windows10.0-KB3157663-x64.cab`  

## <a name="managing-updates-in-nano-server"></a>Gestione degli aggiornamenti in Nano Server

Al momento è possibile usare il provider di Windows Update per Strumentazione gestione Windows (WMI) per trovare l'elenco degli aggiornamenti applicabili e quindi installare tutti o un sottoinsieme di essi. Se si usa Windows Server Update Services (WSUS), è anche possibile configurare Nano Server in modo da ottenere gli aggiornamenti contattando il server WSUS.  

In tutti i casi, è necessario prima definire una sessione remota di Windows PowerShell nel computer Nano Server. In questi esempi viene usato *$sess* per la sessione; se si usa un altro elemento, sostituirlo appropriatamente.  


### <a name="view-all-available-updates"></a>Visualizzare tutti gli aggiornamenti disponibili  
---  
È possibile ottenere l'elenco completo degli aggiornamenti applicabili con i comandi seguenti:  
```  
$sess = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  

$scanResults = Invoke-CimMethod -InputObject $sess -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=0";OnlineScan=$true}  
```  
**Nota:**  
Se non è disponibile alcun aggiornamento, questo comando restituirà l'errore seguente:  
```  
Invoke-CimMethod : A general error occurred that is not covered by a more specific error code.  

At line:1 char:16  

+ ... anResults = Invoke-CimMethod -InputObject $sess -MethodName ScanForUp ...  

+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~  

    + CategoryInfo          : NotSpecified: (MSFT_WUOperatio...-5b842a3dd45d")  

   :CimInstance) [Invoke-CimMethod], CimException  

    + FullyQualifiedErrorId : MI RESULT 1,Microsoft.Management.Infrastructure.  

   CimCmdlets.InvokeCimMethodCommand  
```  

### <a name="install-all-available-updates"></a>Installare tutti gli aggiornamenti disponibili  
---  
È possibile rilevare, scaricare e installare contemporaneamente **tutti** gli aggiornamenti disponibili usando i comandi seguenti:  

```  
$sess = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  

$scanResults = Invoke-CimMethod -InputObject $sess -MethodName ApplyApplicableUpdates  

Restart-Computer  
```  
**Nota:**  
Windows Defender impedirà l'installazione degli aggiornamenti. Per risolvere questo problema, disinstallare Windows Defender, installare gli aggiornamenti e quindi reinstallare Windows Defender. In alternativa, è possibile scaricare gli aggiornamenti in un altro computer, copiarli in Nano Server e quindi applicarli con DISM.exe.  


### <a name="verify-installation-of-updates"></a>Verificare l'installazione degli aggiornamenti  
---  
Usare i comandi seguenti per ottenere un elenco degli aggiornamenti installati:  
```  
$sess = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  

$scanResults = Invoke-CimMethod -InputObject $sess -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=1";OnlineScan=$true}  
```  

**Nota:**  
Questi comandi elencano gli aggiornamenti installati, senza tuttavia specificare "installato" nell'output. Se è necessario un output che includa questa indicazione, ad esempio per un report, è possibile eseguire  
```  
Get-WindowsPackage--Online  
```  

### <a name="using-wsus"></a>Uso di WSUS  
---  
I comandi sopra elencati eseguiranno una query sui servizi Windows Update e Microsoft Update in Internet per trovare e scaricare gli aggiornamenti. Se si usa WSUS, è possibile impostare le chiavi del Registro di sistema in Nano Server in modo da usare il proprio server WSUS.  
  
Vedere la tabella delle "chiavi del Registro di sistema per le opzioni dell'Agente di Windows Update" nell'articolo sulla [configurazione di aggiornamenti automatici in un ambiente non Active Directory](https://technet.microsoft.com/library/cc708449(v=ws.10).aspx).  
  
È necessario impostare almeno le chiavi del Registro di sistema **WUServer** e **WUStatusServer**, ma in base al tipo di implementazione di WSUS possono essere necessari anche altri valori. In tutti i casi, è possibile confermare le impostazioni esaminando un altro Windows Server nello stesso ambiente.  

Dopo aver impostato questi valori per il proprio server WSUS, i comandi specificati nella sezione precedente eseguiranno una query sul server per trovare gli aggiornamenti e usarlo come origine di download.  

### <a name="automatic-updates"></a>Aggiornamenti automatici  
---  
Attualmente, l'unico modo per automatizzare l'installazione degli aggiornamenti è quello di convertire i passaggi precedenti in uno script di Windows PowerShell locale e quindi creare un'attività pianificata per eseguirlo e riavviare il sistema secondo la pianificazione desiderata.


## <a name="performance-and-event-monitoring-on-nano-server"></a>Monitoraggio delle prestazioni e degli eventi in Nano Server
[comment]: # (da Venkat Yalla.)
Nano Server supporta completamente il framework [Event Tracing for Windows](https://aka.ms/u2pa0i) (ETW), ma alcuni degli strumenti familiari usati per gestire i contatori delle prestazioni e le funzionalità di traccia non sono attualmente disponibili in Nano Server. Sono tuttavia disponibili strumenti e cmdlet per eseguire i più comuni scenari di analisi delle prestazioni.

Il flusso di lavoro di alto livello rimane invariato per tutte le installazioni di Windows Server: sul computer di destinazione (Nano Server) viene eseguita un'operazione di traccia con sovraccarico ridotto e sui log e/o sui file di traccia risultanti viene eseguita la post-elaborazione offline in un computer separato tramite strumenti come [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/hardware/hh448170.aspx), [Message Analyzer](https://www.microsoft.com/download/details.aspx?id=44226) o altri.

> [!NOTE]
> Fare riferimento a [How to copy files to and from Nano Server](https://aka.ms/nri9c8) (Come copiare file in e da Nano Server) per un aggiornamento su come trasferire file usando la comunicazione remota di PowerShell.

Nelle sezioni seguenti vengono elencate le più comuni attività di raccolta dei dati sulle prestazioni e viene descritto come eseguirle in Nano Server.

### <a name="query-available-event-providers"></a>Eseguire query su provider di eventi disponibili
[Windows Performance Recorder](https://msdn.microsoft.com/en-us/library/hh448229.aspx) è uno strumento che consente di eseguire query sui provider di eventi disponibili, come illustrato di seguito:
```
wpr.exe -providers
```

È possibile filtrare l'output in base al tipo di eventi di proprio interesse. Ad esempio: 
```
PS C:\> wpr.exe -providers | select-string "Storage"

       595f33ea-d4af-4f4d-b4dd-9dacdd17fc6e                              : Microsoft-Windows-StorageManagement-WSP-Host
       595f7f52-c90a-4026-a125-8eb5e083f15e                              : Microsoft-Windows-StorageSpaces-Driver
       69c8ca7e-1adf-472b-ba4c-a0485986b9f6                              : Microsoft-Windows-StorageSpaces-SpaceManager
       7e58e69a-e361-4f06-b880-ad2f4b64c944                              : Microsoft-Windows-StorageManagement
       88c09888-118d-48fc-8863-e1c6d39ca4df                              : Microsoft-Windows-StorageManagement-WSP-Spaces
```

### <a name="record-traces-from-a-single-etw-provider"></a>Registrare tracce da un unico provider ETW
A questo scopo è possibile usare i nuovi [cmdlet di gestione della traccia eventi](https://technet.microsoft.com/library/dn919247.aspx). Ecco un flusso di lavoro di esempio:

Creare e avviare la traccia, specificando un nome di file per l'archiviazione degli eventi.
```
PS C:\> New-EtwTraceSession -Name "ExampleTrace" -LocalFilePath c:\etrace.etl
```

Aggiungere un GUID di provider alla traccia. Usare ```wpr.exe -providers``` per convertire il nome provider in un GUID. 
```
PS C:\> wpr.exe -providers | select-string "Kernel-Memory"

       d1d93ef7-e1f2-4f45-9943-03d245fe6c00                              : Microsoft-Windows-Kernel-Memory

PS C:\> Add-EtwTraceProvider -Guid "{d1d93ef7-e1f2-4f45-9943-03d245fe6c00}" -SessionName "ExampleTrace"
```

Rimuovere la traccia: viene interrotta la sessione di traccia e gli eventi vengono scaricati nel file di log associato.
```
PS C:\> Remove-EtwTraceSession -Name "ExampleTrace"

PS C:\> dir .\etrace.etl

    Directory: C:\

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/14/2016  11:17 AM       16515072 etrace.etl
```
> [!NOTE]
> In questo esempio viene illustrato come aggiungere un singolo provider di traccia alla sessione, ma è possibile anche usare il cmdlet ```Add-EtwTraceProvider``` più volte in una sessione di traccia con GUID di provider diversi per abilitare operazioni di traccia da più origini. In alternativa è possibile usare i profili ```wpr.exe``` descritti di seguito.

### <a name="record-traces-from-multiple-etw-providers"></a>Registrare tracce da più provider ETW
L'opzione ```-profiles``` di [Windows Performance Recorder](https://msdn.microsoft.com/library/hh448229.aspx) abilita la traccia contemporanea da più provider. È possibile scegliere tra più profili integrati, tra cui CPU, Network e DiskIO:
```
PS C:\Users\Administrator\Documents> wpr.exe -profiles 

Microsoft Windows Performance Recorder Version 10.0.14393 (CoreSystem)
Copyright (c) 2015 Microsoft Corporation. All rights reserved.

        GeneralProfile              First level triage
        CPU                         CPU usage
        DiskIO                      Disk I/O activity
        FileIO                      File I/O activity
        Registry                    Registry I/O activity
        Network                     Networking I/O activity
        Heap                        Heap usage
        Pool                        Pool usage
        VirtualAllocation           VirtualAlloc usage
        Audio                       Audio glitches
        Video                       Video glitches
        Power                       Power usage
        InternetExplorer            Internet Explorer
        EdgeBrowser                 Edge Browser
        Minifilter                  Minifilter I/O activity
        GPU                         GPU activity
        Handle                      Handle usage
        XAMLActivity                XAML activity
        HTMLActivity                HTML activity
        DesktopComposition          Desktop composition activity
        XAMLAppResponsiveness       XAML App Responsiveness analysis
        HTMLResponsiveness          HTML Responsiveness analysis
        ReferenceSet                Reference Set analysis
        ResidentSet                 Resident Set analysis
        XAMLHTMLAppMemoryAnalysis   XAML/HTML application memory analysis
        UTC                         UTC Scenarios
        DotNET                      .NET Activity
        WdfTraceLoggingProvider     WDF Driver Activity
```

Per istruzioni dettagliate sulla creazione di profili personalizzati, vedere la [documentazione su WPR.exe](https://msdn.microsoft.com/library/windows/hardware/hh448223.aspx).

### <a name="record-etw-traces-during-operating-system-boot-time"></a>Registrare tracce ETW durante l'avvio del sistema operativo
Usare il cmdlet ```New-AutologgerConfig``` per raccogliere eventi durante l'avvio del sistema. Può essere usato in modo molto simile al cmdlet ```New-EtwTraceSession```, ma i provider aggiunti alla configurazione di Autologger verranno abilitati solo nella fase iniziale dell'avvio successivo. Il flusso di lavoro complessivo è simile al seguente:

Creare una nuova configurazione di Autologger.
```
PS C:\> New-AutologgerConfig -Name "BootPnpLog" -LocalFilePath c:\bootpnp.etl 
```

Aggiungere ad essa un provider ETW. In questo esempio viene usato il provider PnP del kernel. Richiamare di nuovo ```Add-EtwTraceProvider```, specificando lo stesso nome di Autologger ma un GUID diverso per abilitare la raccolta delle tracce di avvio da più origini.
```
Add-EtwTraceProvider -Guid "{9c205a39-1250-487d-abd7-e831c6290539}" -AutologgerName BootPnpLog
```

Questo comando non avvia immediatamente una sessione ETW, ma ne configura una in modo che venga avviata all'avvio successivo. Dopo il riavvio, viene automaticamente avviata una sessione ETW con il nome della configurazione di Autologger e con i provider di traccia abilitati. Dopo l'avvio di Nano Server, il comando seguente interromperà la sessione di traccia dopo aver scaricato gli eventi registrati nel file di traccia associato:
```
PS C:\> Remove-EtwTraceSession -Name BootPnpLog
```

Per impedire la creazione automatica di un'altra sessione di traccia all'avvio successivo, rimuovere la configurazione di Autologger come indicato di seguito:
```
PS C:\> Remove-AutologgerConfig -Name BootPnpLog
```

Per raccogliere tracce di avvio e di configurazione da più sistemi o da un sistema senza dischi, valutare l'opportunità di usare il [programma di installazione e la raccolta degli eventi di avvio](../administration/get-started-with-setup-and-boot-event-collection.md).

### <a name="capture-performance-counter-data"></a>Acquisire i dati dei contatori delle prestazioni
In genere, i dati dei contatori delle prestazioni vengono monitorati con l'interfaccia grafica Perfmon.exe. In Nano Server usare l'equivalente da riga di comando ```Typeperf.exe```. Ad esempio: 

Eseguire query sui contatori disponibili: è possibile filtrare l'output per trovare più facilmente quelli di proprio interesse.
```
PS C:\> typeperf.exe -q | Select-String "UDPv6"

\UDPv6\Datagrams/sec
\UDPv6\Datagrams Received/sec
\UDPv6\Datagrams No Port/sec
\UDPv6\Datagrams Received Errors
\UDPv6\Datagrams Sent/sec
```

Sono disponibili alcune opzioni che consentono di specificare il numero di volte che devono essere raccolti i valori dei contatori e con quale intervallo. Nell'esempio seguente il valore relativo al tempo di inattività del processore viene raccolto cinque volte ogni tre secondi.
```
PS C:\> typeperf.exe "\Processor Information(0,0)\% Idle Time" -si 3 -sc 5

"(PDH-CSV 4.0)","\\ns-g2\Processor Information(0,0)\% Idle Time"
"09/15/2016 09:20:56.002","99.982990"
"09/15/2016 09:20:59.002","99.469634"
"09/15/2016 09:21:02.003","99.990081"
"09/15/2016 09:21:05.003","99.990454"
"09/15/2016 09:21:08.003","99.998577"
Exiting, please wait...
The command completed successfully.
```

Altre opzioni da riga di comando consentono di specificare i nomi dei contatori delle prestazioni di proprio interesse in un file di configurazione, reindirizzando tra l'altro l'output su un file di log. Per informazioni dettagliate, vedere la [documentazione su typeperf.exe](https://technet.microsoft.com/library/bb490960.aspx).

Con destinazioni Nano Server è possibile anche usare l'interfaccia grafica di Perfmon.exe in modalità remota. Quando si aggiunge un contatore delle prestazioni alla visualizzazione, specificare nel nome computer la destinazione Nano Server anziché il valore predefinito *<Local computer>*.

### <a name="interact-with-the-windows-event-log"></a>Interagire con il registro eventi di Windows

Nano Server supporta il cmdlet ```Get-WinEvent```, che offre funzionalità di query e di filtro per il registro eventi di Windows, sia in locale che su un computer remoto. Esempi e opzioni dettagliate sono disponibili nella [pagina della documentazione di Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx). Questo semplice esempio recupera gli *errori* annotati nel registro di *sistema* nel corso degli ultimi due giorni.
```
PS C:\> $StartTime = (Get-Date) - (New-TimeSpan -Day 2)
PS C:\> Get-WinEvent -FilterHashTable @{LogName='System'; Level=2; StartTime=$StartTime} | select TimeCreated, Message

TimeCreated           Message
-----------           -------
9/15/2016 11:31:19 AM Task Scheduler service failed to start Task Compatibility module. Tasks may not be able to reg...
9/15/2016 11:31:16 AM The Virtualization Based Security enablement policy check at phase 6 failed with status: {File...
9/15/2016 11:31:16 AM The Virtualization Based Security enablement policy check at phase 0 failed with status: {File...
```

Nano Server supporta anche ```wevtutil.exe```, che consente di recuperare informazioni sulle entità di pubblicazione e sui registri eventi. Per informazioni dettagliate, vedere la [documentazione su wevtutil.exe](https://aka.ms/qvod7p). 

### <a name="graphical-interface-tools"></a>Strumenti di interfaccia grafica
Gli [strumenti di gestione server basati sul Web](https://blogs.technet.microsoft.com/servermanagement/2016/08/17/deploy-setup-server-management-tools/) consentono di gestire destinazioni Nano Server e presentare un registro eventi di Nano Server in remoto usando un Web browser. Visualizzatore eventi (eventvwr.msc), uno snap-in di MMC, consente infine di visualizzare i registri: è sufficiente aprirlo in un computer con un desktop e indirizzarlo a un computer Nano Server remoto.




## <a name="using-windows-powershell-desired-state-configuration-with-nano-server"></a>Uso del servizio di configurazione dello stato desiderato di Windows PowerShell con Nano Server  
  
È possibile gestire Nano Server come nodi di destinazione con il servizio di configurazione dello stato desiderato di Windows PowerShell. Attualmente, è possibile gestire i nodi che eseguono Nano Server con il servizio di configurazione dello stato desiderato solo in modalità push. Non tutte le funzionalità possono essere usate con il servizio di configurazione dello stato desiderato con Nano Server.  
  
Per informazioni dettagliate, vedere [Uso di DSC in Nano Server](https://msdn.microsoft.com/powershell/dsc/nanoDsc).  
