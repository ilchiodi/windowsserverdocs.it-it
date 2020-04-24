---
title: PowerShell in Nano Server
description: Differenze nel set ridotto di funzionalità di PowerShell in Nano Server
ms.prod: windows-server
manager: DonGill
ms.technology: server-nano
ms.topic: article
ms.assetid: 9b25b939-1e2c-4bed-a8d3-2a8e8e46b53d
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 4879ae58c24596d64d24b6bece54d4c35837f00f
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826764"
---
# <a name="powershell-on-nano-server"></a>PowerShell in Nano Server

> Si applica a: Windows Server 2016

> [!IMPORTANT]
> A partire da Windows Server versione 1709, Nano Server sarà disponibile solo come [immagine del sistema operativo di base del contenitore](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Per informazioni, vedi [Modifiche apportate a Nano Server](nano-in-semi-annual-channel.md).

## <a name="powershell-editions"></a>Edizioni di PowerShell

A partire dalla versione 5.1, PowerShell è disponibile in diverse edizioni che indicano diversi set di funzionalità e compatibilità della piattaforma.

- **Desktop Edition:** basata su .NET Framework, fornisce la compatibilità con script e moduli destinati a versioni di PowerShell che eseguono edizioni a impatto completo di Windows, ad esempio i componenti di base del server e Windows Desktop.
- **Core Edition:** basata su .NET Core, fornisce la compatibilità con script e moduli destinati a versioni di PowerShell che eseguono edizioni a impatto ridotto di Windows, ad esempio Nano Server e Windows IoT.

L'edizione di PowerShell in esecuzione viene visualizzata nella proprietà PSEdition di $PSVersionTable.
```powershell
$PSVersionTable

Name                           Value
----                           -----
PSVersion                      5.1.14300.1000
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
CLRVersion                     4.0.30319.42000
BuildVersion                   10.0.14300.1000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```

Gli autori di moduli possono dichiarare i propri moduli per la compatibilità con una o più edizioni di PowerShell tramite la chiave manifesto del modulo CompatiblePSEditions. Questa chiave è supportata solo su PowerShell 5.1 o versione successiva.
```powershell
New-ModuleManifest -Path .\TestModuleWithEdition.psd1 -CompatiblePSEditions Desktop,Core -PowerShellVersion 5.1
$moduleInfo = Test-ModuleManifest -Path \TestModuleWithEdition.psd1
$moduleInfo.CompatiblePSEditions
Desktop
Core

$moduleInfo | Get-Member CompatiblePSEditions

   TypeName: System.Management.Automation.PSModuleInfo

Name                 MemberType Definition
----                 ---------- ----------
CompatiblePSEditions Property   System.Collections.Generic.IEnumerable[string] CompatiblePSEditions {get;}

```
Quando si recupera un elenco dei moduli disponibili, è possibile filtrare l'elenco in base all'edizione di PowerShell.
```powershell
Get-Module -ListAvailable | ? CompatiblePSEditions -Contains Desktop

    Directory: C:\Program Files\WindowsPowerShell\Modules


ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   1.0        ModuleWithPSEditions

Get-Module -ListAvailable | ? CompatiblePSEditions -Contains Core | % CompatiblePSEditions
Desktop
Core

```
Gli autori di script possono impedire l'esecuzione di uno script a meno che non venga eseguito in un'edizione compatibile di PowerShell tramite il parametro PSEdition in un'istruzione #requires.
```powershell
Set-Content C:\script.ps1 -Value #requires -PSEdition Core
Get-Process -Name PowerShell
Get-Content C:\script.ps1
#requires -PSEdition Core
Get-Process -Name PowerShell

C:\script.ps1
C:\script.ps1 : The script 'script.ps1' cannot be run because it contained a #requires statement for PowerShell editions 'Core'. The edition of PowerShell that is required by the script does not match the currently running PowerShell Desktop edition.
At line:1 char:1
+ C:\script.ps1
+ ~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (script.ps1:String) [], RuntimeException
    + FullyQualifiedErrorId : ScriptRequiresUnmatchedPSEdition
```

## <a name="differences-in-powershell-on-nano-server"></a>Differenze di PowerShell in Nano Server
PowerShell Core è incluso, per impostazione predefinita, in tutte le installazioni di Nano Server. PowerShell Core è un'edizione a impatto ridotto di PowerShell basata su .NET Core ed eseguita su edizioni a impatto ridotto di Windows, ad esempio Nano Server e Windows IoT Core. PowerShell Core agisce nello stesso modo delle altre edizioni di PowerShell, ad esempio Windows PowerShell in esecuzione su Windows Server 2016. Tuttavia, l'impatto ridotto di Nano Server significa che non tutte le funzionalità di PowerShell da Windows Server 2016 sono disponibili in PowerShell Core su Nano server.


**Funzionalità di Windows PowerShell non disponibili in Nano Server**
* Adattatori di tipo ADSI, ADO e WMI
* Enable-PSRemoting, Disable-PSRemoting (la comunicazione remota di PowerShell è attivata per impostazione predefinita. Vedi la sezione sull'uso della comunicazione remota di Windows PowerShell in [Installare Nano Server](Getting-Started-with-Nano-Server.md)).
* Processi pianificati e modulo PSScheduledJob
* Cmdlet del computer per l'aggiunta a un dominio { Add | Remove }. Per i vari metodi per aggiungere Nano Server a un dominio, vedi la sezione sull'aggiunta di Nano Server a un dominio in [Installare Nano Server](Getting-Started-with-Nano-Server.md).
* Reset-ComputerMachinePassword, Test-ComputerSecureChannel
* Profili (è possibile aggiungere uno script di avvio per le connessioni remote in ingresso con `Set-PSSessionConfiguration`)
* Cmdlet Clipboard
* Cmdlet EventLog { Clear | Get | Limit | New | Remove | Show | Write } (usare in alternativa i cmdlet New-WinEvent e Get-WinEvent).
* Cmdlet Get-PfxCertificate
* Cmdlet TraceSource { Get | Set }
* Cmdlet Counter { Get | Export | Import }
* Alcuni cmdlet relativi al web { New-WebServiceProxy, Send-MailMessage, ConvertTo-Html }
* Registrazione e traccia usando il modulo PSDiagnostics
* Get-HotFix (per ottenere e gestire gli aggiornamenti in Nano Server, vedere [Gestire Nano Server](Manage-Nano-Server.md)).
* Cmdlet di comunicazione remota implicita { Export-PSSession | Import-PSSession }
* New-PSTransportOption
* Transazioni PowerShell e cmdlet delle transazioni { Complete | Get | Start | Undo | Use }
* Cmdlet, moduli e infrastruttura del flusso di lavoro di PowerShell
* Out-Printer
* Update-List
* Cmdlet WMI versione 1: Get-WmiObject, Invoke-WmiMethod, Register-WmiEvent, Remove-WmiObject, Set-WmiInstance (usa in alternativa il cmdlet CimCmdlet).

## <a name="using-windows-powershell-desired-state-configuration-with-nano-server"></a>Uso del servizio di configurazione dello stato desiderato di Windows PowerShell con Nano Server

È possibile gestire Nano Server come nodi di destinazione con il servizio di configurazione dello stato desiderato di Windows PowerShell. Attualmente, è possibile gestire i nodi che eseguono Nano Server con il servizio di configurazione dello stato desiderato solo in modalità push. Non tutte le funzionalità possono essere usate con il servizio di configurazione dello stato desiderato con Nano Server.

Per informazioni dettagliate, vedere [Uso di DSC in Nano Server](https://docs.microsoft.com/powershell/scripting/dsc/getting-started/nanodsc).

