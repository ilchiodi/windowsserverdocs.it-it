---
title: Raccogliere i dati di diagnostica con Spazi di archiviazione diretta
description: Informazioni sugli strumenti di raccolta dati Spazi di archiviazione diretta, con esempi specifici su come eseguirli e usarli.
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 10/24/2018
ms.openlocfilehash: 75a74017f48b357dd029b062a7ce06775836bd0a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858964"
---
# <a name="collect-diagnostic-data-with-storage-spaces-direct"></a>Raccogliere i dati di diagnostica con Spazi di archiviazione diretta

> Si applica a: Windows Server 2019, Windows Server 2016

Sono disponibili diversi strumenti di diagnostica che possono essere utilizzati per raccogliere i dati necessari per la risoluzione dei problemi relativi a Spazi di archiviazione diretta e cluster di failover. In questo articolo si incentrerà su **Get-SDDCDiagnosticInfo** -uno strumento One Touch che raccoglie tutte le informazioni rilevanti per facilitare la diagnosi del cluster.

Dato che i log e altre informazioni su **Get-SDDCDiagnosticInfo** sono densi, le informazioni sulla risoluzione dei problemi illustrate di seguito saranno utili per la risoluzione dei problemi avanzati che sono stati escalati e che potrebbero richiedere l'invio di dati a Microsoft per la valutazione.

## <a name="installing-get-sddcdiagnosticinfo"></a>Installazione di Get-SDDCDiagnosticInfo

Cmdlet di PowerShell **Get-SDDCDiagnosticInfo** (noto anche come **Get-PCStorageDiagnosticInfo**, noto in precedenza come **test-StorageHealth**, può essere usato per raccogliere i log ed eseguire i controlli di integrità per il clustering di failover (cluster, risorse, reti, nodi), spazi di archiviazione (dischi fisici, enclosure, dischi virtuali), volumi condivisi cluster, condivisioni file SMB e deduplicazione. 

Sono disponibili due metodi di installazione dello script, entrambi delineati di seguito.

### <a name="powershell-gallery"></a>PowerShell Gallery

Il [PowerShell Gallery](https://www.powershellgallery.com/packages/PrivateCloud.DiagnosticInfo) è uno snapshot del repository GitHub. Si noti che l'installazione di elementi dal PowerShell Gallery richiede la versione più recente del modulo PowerShellGet, disponibile in Windows 10, in Windows Management Framework (WMF) 5,0 o nel programma di installazione basato su MSI (per PowerShell 3 e 4).

Durante questo processo viene installata anche la versione più recente degli [strumenti di diagnostica di rete Microsoft](https://www.powershellgallery.com/packages/MSFT.Network.Diag) , poiché Get-SDDCDiagnosticInfo si basa su questo. Questo modulo del manifesto contiene lo strumento di diagnostica e risoluzione dei problemi di rete, gestito dal gruppo di prodotti Microsoft Core Networking in Microsoft.

È possibile installare il modulo eseguendo il comando seguente in PowerShell con privilegi di amministratore:

``` PowerShell
Install-PackageProvider NuGet -Force
Install-Module PrivateCloud.DiagnosticInfo -Force
Import-Module PrivateCloud.DiagnosticInfo -Force
Install-Module -Name MSFT.Network.Diag
```

Per aggiornare il modulo, eseguire il comando seguente in PowerShell:

``` PowerShell
Update-Module PrivateCloud.DiagnosticInfo
```

### <a name="github"></a>GitHub

Il [repository GitHub](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/) è la versione più aggiornata del modulo, dal momento che l'iterazione continua qui. Per installare il modulo da GitHub, scaricare il modulo più recente dall' [Archivio](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/archive/master.zip) ed estrarre la directory PrivateCloud. DiagnosticInfo nel percorso dei moduli di PowerShell corretto indicato da ```$env:PSModulePath```

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

Se è necessario ottenere questo modulo in un cluster offline, scaricare il file zip, spostarlo nel nodo del server di destinazione e installare il modulo.

## <a name="gathering-logs"></a>Raccolta dei log

Dopo aver abilitato i canali eventi e completato il processo di installazione, è possibile usare il cmdlet di PowerShell Get-SDDCDiagnosticInfo nel modulo per ottenere:

- Report sull'integrità dell'archiviazione e dettagli sui componenti non integri
- Report sulla capacità di archiviazione in base al pool, al volume e al volume deduplicato
- Log eventi da tutti i nodi del cluster e una segnalazione errori di riepilogo

Si supponga che il cluster di archiviazione abbia il nome *"clus01".*

Per l'esecuzione in un cluster di archiviazione remoto:

``` PowerShell
Get-SDDCDiagnosticInfo -ClusterName CLUS01
```

Per eseguire localmente nel nodo di archiviazione in cluster:

``` PowerShell
Get-SDDCDiagnosticInfo
```

Per salvare i risultati in una cartella specificata:

``` PowerShell
Get-SDDCDiagnosticInfo -WriteToPath D:\Folder 
```

Di seguito è riportato un esempio di come viene eseguita la ricerca in un cluster reale:

``` PowerShell
New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
Get-SddcDiagnosticInfo -ClusterName S2D-Cluster -WriteToPath d:\SDDCDiagTemp
```

Come si può notare, lo script eseguirà anche la convalida dello stato corrente del cluster

![schermata di PowerShell per la raccolta dati](media/data-collection/CollectData.png)

Come si può notare, tutti i dati vengono scritti nella cartella SDDCDiagTemp

![schermata dei dati in Esplora file](media/data-collection/CollectDataFolder.png)

Al termine dell'esecuzione dello script, verrà creato un file ZIP nella directory degli utenti

![schermata di data zip in PowerShell](media/data-collection/CollectDataResult.png)

Generazione di un report in un file di testo

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

Per riferimento, di seguito è riportato un collegamento al [report di esempio](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/SDDCReport.txt) e al file zip di [esempio](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/HealthTest-S2D-Cluster-20180522-1546.ZIP).

Per visualizzarlo nell'interfaccia di amministrazione di Windows (versione 1812 e successive), passare alla scheda *diagnostica* . Come è possibile osservare nella schermata seguente, è possibile 

- Installare gli strumenti di diagnostica
- Aggiornare i dati (se non sono aggiornati), 
- Pianificare le esecuzioni di diagnostica giornaliere, che hanno un basso effetto sul sistema, in genere < 5 minuti in background e non richiede più di 500MB nel cluster.
- Consente di visualizzare le informazioni di diagnostica raccolte in precedenza se è necessario fornirle per supportarle o analizzarle autonomamente.

![schermata di diagnostica WAC](media/data-collection/Wac.png)

## <a name="get-sddcdiagnosticinfo-output"></a>Output di Get-SDDCDiagnosticInfo

Di seguito sono riportati i file inclusi nell'output compresso di Get-SDDCDiagnosticInfo.

### <a name="health-summary-report"></a>Report Riepilogo integrità

Il report di riepilogo integrità viene salvato come:
- 0_CloudHealthSummary. log

Questo file viene generato dopo aver analizzato tutti i dati raccolti ed è destinato a fornire un breve riepilogo del sistema. Contiene:

- Informazioni sistema
- Panoramica sull'integrità dell'archiviazione (numero di nodi, risorse online, volumi condivisi del cluster online, componenti non integri e così via)
- Dettagli sui componenti non integri (risorse cluster offline, non riuscite o in sospeso online)
- Informazioni sul firmware e sul driver
- Dettagli del pool, del disco fisico e del volume
- Prestazioni di archiviazione (vengono raccolti i contatori delle prestazioni)

Questo report viene aggiornato continuamente per includere informazioni più utili. Per informazioni aggiornate, vedere il [file Leggimi di GitHub](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/edit/master/README.md).

### <a name="logs-and-xml-files"></a>Log e file XML

Lo script esegue vari script di raccolta dei log e salva l'output come file XML. Vengono raccolti i log di cluster e di integrità, le informazioni di sistema (MSInfo32), i registri eventi non filtrati (clustering di failover, diagnostica dis, Hyper-v, spazi di archiviazione e così via) e le informazioni di diagnostica di archiviazione (log operativi). Per informazioni aggiornate sulle informazioni raccolte, vedere il [file Leggimi di GitHub (raccolta)](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/blob/master/README.md#what-does-the-cmdlet-output-include).

## <a name="how-to-consume-the-xml-files-from-get-pcstoragediagnosticinfo"></a>Come utilizzare i file XML da Get-PCStorageDiagnosticInfo
È possibile utilizzare i dati dei file XML forniti nei dati raccolti dal cmdlet **Get-PCStorageDiagnosticInfo** . Questi file contengono informazioni sui dischi virtuali, i dischi fisici, le informazioni di base sul cluster e altri output correlati a PowerShell. 

Per visualizzare i risultati di questi output, aprire una finestra di PowerShell ed eseguire i passaggi seguenti. 

```PowerShell
ipmo storage
$d = import-clixml <filename> 
$d
```

## <a name="what-to-expect-next"></a>Cosa aspettarsi successivamente?
Sono stati apportati numerosi miglioramenti e nuovi cmdlet per analizzare l'integrità del sistema SDDC.
Inviare commenti e suggerimenti su ciò che si desidera visualizzare segnalando i problemi [qui](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/issues). Inoltre, è possibile contribuire a apportare modifiche utili allo script inviando una [richiesta pull](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/pulls).
