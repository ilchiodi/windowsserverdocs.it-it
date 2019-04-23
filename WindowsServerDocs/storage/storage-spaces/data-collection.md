---
title: Raccogliere dati diagnostici con spazi di archiviazione diretta
description: Informazioni sugli spazi diretto dei dati raccolta strumenti di archiviazione, con esempi specifici di come eseguire e usarli.
keywords: Spazi di archiviazione, la raccolta dei dati, risoluzione dei problemi, i canali di eventi, Get-SDDCDiagnosticInfo
ms.assetid: ''
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 10/24/2018
ms.localizationpriority: ''
ms.openlocfilehash: eaa7d92fe6f77697614cacf1405a25e5a42e14b7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880272"
---
# <a name="collect-diagnostic-data-with-storage-spaces-direct"></a>Raccogliere dati diagnostici con spazi di archiviazione diretta

> Si applica a: Windows Server 2019, Windows Server 2016

Sono disponibili vari strumenti di diagnostica che possono essere utilizzati per raccogliere i dati necessari per risolvere i problemi di spazi di archiviazione diretta e Cluster di Failover. In questo articolo esamineremo **Get-SDDCDiagnosticInfo** -uno strumento di un dispositivo a tocco che raccoglierà tutte le informazioni pertinenti per aiutare a diagnosticare il cluster.

<!-- The health summary report is a great start to understanding the status of your system to start diagnosing an issue. -->

Dato che i log e altre informazioni che **Get-SDDCDiagnosticInfo** sono di tipo densa, le informazioni sulla risoluzione dei problemi riportati di seguito saranno utili per la risoluzione dei problemi avanzata problemi che sono stati inoltrati e che può richiedere i dati da inviare a Microsoft per la valutazione.

<!--
## Collecting live dumps

Windows will trigger the collection of a ``` LiveDump ``` when there are known resources that are hanging in kernel calls. ``` RHS ``` will trigger ```LiveDump``` collection if both the resource type and cluster ``` DumpPolicy ``` are set to 1. For physical disk it is set out of the box
-->

## <a name="installing-get-sddcdiagnosticinfo"></a>Installazione di Get-SDDCDiagnosticInfo

Il **Get-SDDCDiagnosticInfo** cmdlet di PowerShell (noto anche come **Get-PCStorageDiagnosticInfo**, precedentemente noto come **Test StorageHealth**) può essere usato per raccogliere i log per ed eseguire controlli di integrità per il Clustering di Failover (Cluster, le risorse, reti, i nodi), (spazi di archiviazione I dischi fisici, enclosure, i dischi virtuali) del Cluster condivisi volumi, condivisioni File SMB e la deduplicazione. 

Esistono due metodi di installazione di script, entrambi sono strutture riportato di seguito.

### <a name="powershell-gallery"></a>PowerShell Gallery

Il [PowerShell Gallery](https://www.powershellgallery.com/packages/PrivateCloud.DiagnosticInfo) è uno snapshot del repository GitHub. Si noti che l'installazione degli elementi da PowerShell Gallery richiede la versione più recente del modulo PowerShellGet, disponibile in Windows 10, in Windows Management Framework (WMF) 5.0 o nel programma di installazione basata su MSI (per PowerShell 3 e 4).

È possibile installare il modulo eseguendo il comando seguente in PowerShell con privilegi di amministratore:

``` PowerShell
Install-PackageProvider NuGet -Force
Install-Module PrivateCloud.DiagnosticInfo -Force
Import-Module PrivateCloud.DiagnosticInfo -Force
```

Per aggiornare il modulo, eseguire il comando seguente in PowerShell:

``` PowerShell
Update-Module PrivateCloud.DiagnosticInfo
```

### <a name="github"></a>GitHub

Il [repository GitHub](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/) è la versione più aggiornata del modulo, poiché si sta costantemente l'iterazione di seguito. Per installare il modulo da GitHub, scaricare il modulo più recente dal [archive](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/archive/master.zip) ed estrarre la directory PrivateCloud.DiagnosticInfo sul percorso di moduli di PowerShell corretto a cui fa riferimento ```$env:PSModulePath```

``` PowerShell
# Allowing Tls12 and Tls11 -- e.g. github now requires Tls12
# If this is not set, the Invoke-WebRequest fails with "The request was aborted: Could not create SSL/TLS secure channel."
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$module = 'PrivateCloud.DiagnosticInfo'
Invoke-WebRequest -Uri https://github.com/PowerShell/$module/archive/master.zip -OutFile $env:TEMP\master.zip
Expand-Archive -Path $env:TEMP\master.zip -DestinationPath $env:TEMP -Force
if (Test-Path $env:SystemRoot\System32\WindowsPowerShell\v1.0\Modules\$module) {
    rm -Recurse $env:SystemRoot\System32\WindowsPowerShell\v1.0\Modules\$module -ErrorAction Stop
    Remove-Module $module -ErrorAction SilentlyContinue
} else {
    Import-Module $module -ErrorAction SilentlyContinue
} 
if (-not ($m = Get-Module $module -ErrorAction SilentlyContinue)) {
    $md = "$env:ProgramFiles\WindowsPowerShell\Modules"
} else {
    $md = (gi $m.ModuleBase -ErrorAction SilentlyContinue).PsParentPath
    Remove-Module $module -ErrorAction SilentlyContinue
    rm -Recurse $m.ModuleBase -ErrorAction Stop
}
cp -Recurse $env:TEMP\$module-master\$module $md -Force -ErrorAction Stop
rm -Recurse $env:TEMP\$module-master,$env:TEMP\master.zip
Import-Module $module -Force

``` 

Se è necessario ottenere questo modulo in un cluster non in linea, scaricare il file zip, spostarlo al nodo del server di destinazione e installare il modulo.

## <a name="gathering-logs"></a>Raccolta dei log

Dopo aver abilitato i canali di eventi e completare il processo di installazione, è possibile usare il cmdlet di PowerShell Get-SDDCDiagnosticInfo nel modulo per ottenere:

- Report su integrità di archiviazione, oltre a informazioni dettagliate sui componenti di tipo non integro
- Report di capacità di archiviazione dal pool, volumi e volumi deduplicati
- Registri eventi da tutti i nodi del cluster e un report di riepilogo degli errori

Si supponga che il cluster di archiviazione con il nome *"CLUS01".*

Per l'esecuzione avviene in un cluster di archiviazione remoti:

``` PowerShell
Get-SDDCDiagnosticInfo -ClusterName CLUS01
```

Per eseguire in locale nel nodo di archiviazione in cluster:

``` PowerShell
Get-SDDCDiagnosticInfo
```

Per salvare i risultati in una cartella specificata:

``` PowerShell
Get-SDDCDiagnosticInfo -WriteToPath D:\Folder 
```

Ecco un esempio di questo aspetto in un vero cluster:

``` PowerShell
New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
Get-SddcDiagnosticInfo -ClusterName S2D-Cluster -WriteToPath d:\SDDCDiagTemp
```

Come può notare, lo script eseguirà anche la convalida dello stato corrente del cluster

![screenshot di powershell di raccolta dati](media/data-collection/CollectData.png)

Come può notare, tutti i dati vengono scritti SDDCDiagTemp cartella

![dati nello screenshot di Esplora file](media/data-collection/CollectDataFolder.png)

Dopo che lo script verrà completato e verrà creato con estensione ZIP nella directory degli utenti

![dati zip nello screenshot di powershell](media/data-collection/CollectDataResult.png)

È necessario generare un report in un file di testo

```PowerShell
#find the latest diagnostic zip in UserProfile
    $DiagZip=(get-childitem $env:USERPROFILE | where Name -like HealthTest*.zip)
    $LatestDiagPath=($DiagZip | sort lastwritetime | select -First 1).FullName
#expand to temp directory
    New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
    Expand-Archive -Path $LatestDiagPath -DestinationPath D:\SDDCDiagTemp -Force
#generate report and save to text file
    $report=Show-SddcDiagnosticReport -Path D:\SDDCDiagTemp
    $report | out-file d:\SDDCReport.txt
    
```

Per riferimento, ecco un collegamento per il [report di esempio](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/SDDCReport.txt) e [zip di esempio](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/HealthTest-S2D-Cluster-20180522-1546.ZIP).

Per visualizzare tali informazioni in Windows Admin Center (e versioni successive 1812 versione), passare al *diagnostica* scheda. Come noterete nella schermata seguente, è possibile 

- Installare gli strumenti di diagnostica
- Aggiornamento dei dati (in tal caso non aggiornati), 
- Pianificare le esecuzioni giornaliere di diagnostica (hanno un valore basso impatto sul sistema, in genere richiedere < 5 minuti in background, e non occupa più di 500mb nel cluster)
- Vista precedentemente raccolte le informazioni di diagnostica se si desidera assegnare a supportare o analizzarli personalmente.

![screenshot della diagnostica wac](media/data-collection/Wac.png)

## <a name="get-sddcdiagnosticinfo-output"></a>Output di Get-SDDCDiagnosticInfo

Di seguito sono i file inclusi nell'output di Get-SDDCDiagnosticInfo compresso.

### <a name="health-summary-report"></a>Report di riepilogo dell'integrità

Report di riepilogo dell'integrità viene salvato come:
- 0_CloudHealthSummary.log

Questo file viene generato dopo l'analisi di tutti i dati raccolti e mira a fornire un breve riepilogo del sistema. Contiene:

- Informazioni di sistema
- Panoramica dell'integrità di archiviazione (numero di nodi di backup, le risorse online, volumi condivisi del cluster online, i componenti di tipo non integro e così via).
- Dettagli sui componenti di tipo non integro (risorse cluster sono offline, non riuscite o in linea in sospeso)
- Informazioni di firmware e driver
- Pool di dischi fisici e i dettagli di volume
- Prestazioni di archiviazione (vengono raccolti i contatori delle prestazioni)

Questo report è in corso continuamente aggiornato per includere le informazioni più utili. Per informazioni aggiornate, vedere la [README GitHub](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/edit/master/README.md).

### <a name="logs-and-xml-files"></a>I log e file XML

Lo script esegue vari log di raccolta script e l'output viene salvato come file xml. Microsoft raccoglie i log del cluster e l'integrità, le informazioni di sistema (MSInfo32), non filtrato i registri eventi (clustering di failover, diagnostica dis, hyper-v, spazi di archiviazione e altro ancora) e le informazioni di diagnostica di archiviazione (log operativi). Per informazioni aggiornate sul tipo di informazioni raccolte, vedere la [README GitHub (che cos'è raccogliere)](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/blob/master/README.md#what-does-the-cmdlet-output-include).

<!--
## Enabling event channels

When Windows Server is installed, many event channels are enabled by default. But sometimes when diagnosing an issue, we want to be able to enable some of these event channels since it will help in triaging and diagnosing system issues.

You could enable additional event channels on each server node in your cluster as needed; however, this approach presents two problems:

1. You need to remember to enable the same event channels on every new server node that you add to your cluster.
2. When diagnosing, it can be tedious to enable specific event channels, reproduce the error, and repeat this process until you root cause.

To avoid these issues, you can enable event channels on cluster startup. The list of enabled event channels on your cluster can be configured using the public property **EnabledEventLogs**. By default, the following event channels are enabled:

```powershell
PS C:\Windows\system32> (get-cluster).EnabledEventLogs
```

Here's an example of the output:
```
Microsoft-Windows-Hyper-V-VmSwitch-Diagnostic,4,0xFFFFFFFD
Microsoft-Windows-SMBDirect/Debug,4
Microsoft-Windows-SMBServer/Analytic
Microsoft-Windows-Kernel-LiveDump/Analytic
```

The **EnabledEventLogs** property is a multistring, where each string is in the form: **channel-name, log-level, keyword-mask**. The **keyword-mask** can be a hexadecimal (prefix 0x), octal (prefix 0), or decimal number (no prefix) number that each event contains (so you can filter by it). For instance, to add a new event channel to the list and to configure both **log-level** and **keyword-mask** you can run:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,2,321"
```

If you want to set the **log-level** but keep the **keyword-mask** at its default value, you can use either of the following commands:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,2"
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,2,"
```

If you want to keep the **log-level** at its default value, but set the **keyword-mask** you can run the following command:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,,0xf1"
```

If you want to keep both the **log-level** and the **keyword-mask** at their default values, you can run any of the following commands:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic"
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,"
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,,"
```

These event channels will be enabled on every cluster node when the cluster service starts or whenever the **EnabledEventLogs** property is changed.
-->

## <a name="how-to-consume-the-xml-files-from-get-pcstoragediagnosticinfo"></a>Come usare i file XML da Get-PCStorageDiagnosticInfo
È possibile utilizzare i dati da file XML forniti nei dati raccolti dal **Get-PCStorageDiagnosticInfo** cmdlet. Questi file dispone di informazioni sul virtuale i dischi, dischi fisici, le informazioni di base del cluster e altri PowerShell relativi output. 

Per visualizzare i risultati di questi output, aprire una finestra di PowerShell ed eseguire i passaggi seguenti. 

```PowerShell
ipmo storage
$d = import-clixml <filename> 
$d
```

## <a name="what-to-expect-next"></a>Cosa accade quindi?
Numerosi miglioramenti e nuovi cmdlet per l'analisi dell'integrità del sistema SDDC.
Fornire commenti e suggerimenti su ciò che si desidera vedere compilando problemi [qui](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/issues). Inoltre, non esitate a contribuire con le modifiche utili per lo script, inviando un [richiesta pull](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/pulls).
