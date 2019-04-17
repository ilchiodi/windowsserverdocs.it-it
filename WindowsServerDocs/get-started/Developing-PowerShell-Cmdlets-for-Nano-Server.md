---
title: Sviluppo di cmdlet di PowerShell per Nano Server
description: 'portabilità CIM, cmdlet di .NET, C++ '
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b4267f0-1c91-4a40-9262-5daf4659f686
author: jaimeo
ms.author: jaimeo
ms.date: 09/06/2017
ms.localizationpriority: medium
ms.openlocfilehash: 4c669db414c4f12b6145a26a75b83449f43e8918
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082303"
---
# <a name="developing-powershell-cmdlets-for-nano-server"></a>Sviluppo di cmdlet di PowerShell per Nano Server

>Si applica a: Windows Server 2016

> [!IMPORTANT]
> A partire da Windows Server, versione 1709, Nano Server sarà disponibile solo come [immagine del sistema operativo di base del contenitore](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Per scoprire ciò cosa implica, vedi [Modifiche a Nano Server](nano-in-semi-annual-channel.md). 
  
## <a name="overview"></a>Panoramica  
PowerShell Core è incluso, per impostazione predefinita, in tutte le installazioni di Nano Server. PowerShell Core è un'edizione a impatto ridotto di PowerShell basata su .NET Core ed eseguita su edizioni a impatto ridotto di Windows, ad esempio Nano Server e Windows IoT Core. PowerShell Core agisce nello stesso modo delle altre edizioni di PowerShell, ad esempio Windows PowerShell in esecuzione su Windows Server 2016. Tuttavia, l'impatto ridotto di Nano Server significa che non tutte le funzionalità di PowerShell da Windows Server 2016 sono disponibili in PowerShell Core su Nano server.  
  
Se si hanno cmdlet di PowerShell esistenti che si vuole eseguire su Nano Server o si stanno sviluppando nuovi cmdlet con lo stesso scopo, in questo argomento è possibile trovare indicazioni e suggerimenti per eseguire queste operazioni in modo semplice ed efficiente.  

## <a name="powershell-editions"></a>Edizioni di PowerShell  
  
  
A partire dalla versione 5.1, PowerShell è disponibile in diverse edizioni che indicano diversi set di funzionalità e compatibilità della piattaforma.  
  
- **Desktop Edition:** basata su .NET Framework, fornisce la compatibilità con script e moduli destinati a versioni di PowerShell che eseguono edizioni a impatto completo di Windows, ad esempio Server Core e Windows Desktop.  
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
Get-Module -ListAvailable | ? CompatiblePSEditions -Contains "Desktop"  
  
    Directory: C:\Program Files\WindowsPowerShell\Modules  
  
  
ModuleType Version    Name                                ExportedCommands  
---------- -------    ----                                ----------------  
Manifest   1.0        ModuleWithPSEditions  
  
Get-Module -ListAvailable | ? CompatiblePSEditions -Contains "Core" | % CompatiblePSEditions  
Desktop  
Core  
  
```  
Gli autori di script possono impedire l'esecuzione di uno script a meno che non venga eseguito in un'edizione compatibile di PowerShell tramite il parametro PSEdition in un'istruzione #requires.  
```powershell  
Set-Content C:\script.ps1 -Value "#requires -PSEdition Core  
Get-Process -Name PowerShell"  
Get-Content C:\script.ps1  
#requires -PSEdition Core  
Get-Process -Name PowerShell  
  
C:\script.ps1  
C:\script.ps1 : The script 'script.ps1' cannot be run because it contained a "#requires" statement for PowerShell editions 'Core'. The edition of PowerShell that is required by the script does not match the currently running PowerShell Desktop edition.  
At line:1 char:1  
+ C:\script.ps1  
+ ~~~~~~~~~~~~~  
    + CategoryInfo          : NotSpecified: (script.ps1:String) [], RuntimeException  
    + FullyQualifiedErrorId : ScriptRequiresUnmatchedPSEdition  
```  
  
  
## <a name="installing-nano-server"></a>Installazione di Nano Server  
Una guida di avvio rapido e informazioni dettagliate per l'installazione di Nano Server in macchine fisiche o virtuali sono disponibili nell'argomento [Installare Nano Server](Getting-Started-with-Nano-Server.md), correlato al presente argomento.  
  
> [!NOTE]  
> Per attività di sviluppo su Nano Server può essere utile installare Nano Server usando il parametro -Development di New-NanoServerImage. Questo accorgimento consente infatti di abilitare l'installazione di driver senza firma, copiare i file binari del debugger, aprire una porta per il debug, abilitare la firma di test e consentire l'installazione di pacchetti AppX senza una licenza per sviluppatori. Ad esempio:  
>  
>`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -Development`  
  
## <a name="determining-the-type-of-cmdlet-implementation"></a>Definizione del tipo di implementazione dei cmdlet  
PowerShell supporta vari tipi di implementazione dei cmdlet e, in baso al tipo selezionato, dipendono il processo e gli strumenti necessari per la creazione o la conversione degli stessi in Nano Server. I tipi di implementazione supportati sono:  
* CIM: costituita da file CDXML sovrapposti su provider CIM (WMIv2)   
* .NET: costituita da assembly .NET che implementano interfacce di cmdlet gestite, generalmente scritte in C#   
* Script di PowerShell: costituita da moduli di script (psm1) o script (ps1) scritti nel linguaggio di PowerShell   
  
Se non si è certi del tipo di implementazione usato per i cmdlet esistenti che si vuole convertire, installare il prodotto o la funzionalità e quindi cercare la cartella del modulo di PowerShell in uno dei percorsi seguenti:   
  
* %windir%\system32\WindowsPowerShell\v1.0\Modules   
* %ProgramFiles%\WindowsPowerShell\Modules   
* %UserProfile%\Documents\WindowsPowerShell\Modules   
* \<percorso di installazione del prodotto>   
    
 In questi percorsi cercare i dettagli seguenti:  
 * cmdlet CIM con estensioni di file cdxml.  
 * cmdlet .NET con estensioni di file dll o con assembly installati nella GAC elencata nel file con estensione psd1 nei campi RootModule, ModuleToProcess o NestedModules.  
* cmdlet di script PowerShell con estensioni di file psm1 o ps1.   
  
## <a name="porting-cim-cmdlets"></a>Porting di cmdlet CIM  
In genere, questi cmdlet possono essere usati in Nano Server senza alcuna conversione. È necessario tuttavia eseguire la conversione del provider WMI v2 sottostante per consentirne l'esecuzione su Nano Server (se non è già stato convertito).  
  
### <a name="building-c-for-nano-server"></a>Compilazione di C++ per Nano Server  
Per ottenere DLL C++ ottimizzate per Nano Server, è necessario compilarle per Nano Server anziché per un'edizione specifica.  
  
Per i prerequisiti e procedure dettagliate inerenti allo sviluppo di C++ su Nano Server, vedere [Sviluppo in Nano Server](http://blogs.technet.com/b/nanoserver/archive/2016/04/27/developing-native-apps-on-nano-server.aspx).  
  
  
## <a name="porting-net-cmdlets"></a>Conversione di cmdlet .NET  
In Nano Server è supportata la maggior parte del codice C#. È possibile usare [ApiPort](https://github.com/Microsoft/dotnet-apiport) per cercare le API incompatibili.  
  
### <a name="powershell-core-sdk"></a>Powershell Core SDK  
In [PowerShell Gallery](https://www.powershellgallery.com/packages/Microsoft.PowerShell.NanoServer.SDK/) è disponibile il modulo "Microsoft.PowerShell.NanoServer.SDK", che semplifica lo sviluppo di cmdlet .NET con Visual Studio 2015 Update 2 per le versioni di CoreCLR e PowerShell Core disponibili in Nano Server. È possibile installare il modulo usando PowerShellGet con questo comando:  
  
`Find-Module Microsoft.PowerShell.NanoServer.SDK -Repository PSGallery | Install-Module -Scope <scope>`  
  
Il modulo PowerShell Core SDK espone i cmdlet per impostare gli assembly di riferimento CoreCLR e PowerShell Core corretti, creare un progetto C# in Visual Studio 2015 per gli assembly di riferimento e impostare il debugger remoto in un computer Nano Server in modo che gli sviluppatori possano eseguire in remoto da Visual Studio 2015 il debug dei cmdlet .NET in esecuzione su Nano Server.  
  
Il modulo PowerShell Core SDK richiede Visual Studio 2015 Update 2. Se Visual Studio 2015 non è installato nel sistema, è possibile installare [Visual Studio Community 2015](https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx).  
  
Il modulo SDK richiede anche che in Visual Studio 2015 sia installata la funzionalità seguente:  
  
- Sviluppo per Windows e Web -> Strumenti per lo sviluppo di app di Windows universale -> Strumenti (1.3.1) e Windows 10 SDK  
  
Prima di usare il modulo SDK, rivedere l'installazione di Visual Studio per verificare che questi requisiti siano soddisfatti. Assicurarsi di selezionare la funzionalità sopra indicata durante l'installazione di Visual Studio oppure modificare l'installazione di Visual Studio 2015 esistente per installarla.  
  
Il modulo PowerShell Core SDK include i cmdlet seguenti:  
- New-NanoCSharpProject: crea un nuovo progetto Visual Studio C# per gli assembly CoreCLR e PowerShell Core inclusi nella versione Windows Server 2016 di Nano Server.  
- Show-SdkSetupReadMe: apre la cartella radice di SDK in Esplora file e apre il file README.txt per l'installazione manuale.  
- Install-RemoteDebugger: installa e configura il debugger remoto di Visual Studio in un computer Nano Server.  
- Start-RemoteDebugger: avvia il debugger remoto in un computer remoto che esegue Nano Server.  
- Stop-RemoteDebugger: arresta il debugger remoto in un computer remoto che esegue Nano Server.  
  
Per informazioni dettagliate su come usare i cmdlet, eseguire Get-Help su ogni cmdlet dopo aver installato e importato il modulo, come descritto di seguito:  
  
`Get-Command -Module Microsoft.PowerShell.NanoServer.SDK | Get-Help -Full`   
  
  
### <a name="searching-for-compatible-apis"></a>Ricerca di API compatibili  
  
È possibile eseguire la ricerca di .NET Core nel catalogo API oppure disassemblare gli assembly di riferimento CoreCLR. Per altre informazioni sulla portabilità della piattaforma delle API .NET, vedere [Platform Portability](https://github.com/Microsoft/dotnet-apiport/blob/master/docs/HowTo/PlatformPortability.md) (Portabilità della piattaforma).  
  
### <a name="pinvoke"></a>PInvoke  
Nell'assembly CoreCLR usata da Nano Server, alcune DLL fondamentali come kernel32.dll e advapi32.dll sono state suddivise in più set di API in modo che l'utente debba accertarsi che i PInvoke facciano riferimento all'API corretta. Eventuali incompatibilità produrranno un errore di runtime.  
  
Per un elenco delle API native supportate in Nano Server, vedere l'elenco di [API di Nano Server](https://msdn.microsoft.com/library/mt588480(v=vs.85).aspx).  
  
### <a name="building-c-for-nano-server"></a>Compilazione di C++ per Nano Server  
  
Dopo aver creato un progetto C# in Visual Studio 2015 usando `New-NanoCSharpProject`, è possibile compilarlo in Visual Studio semplicemente facendo clic sul menu **Build** e selezionando **Compila progetto** o **Compila soluzione**. Gli assembly generati avranno come destinazione i moduli CoreCLR e PowerShell Core appropriati di Nano Server e, per usarli, sarà sufficiente copiare gli assembly in un computer che esegue Nano Server.  
  
### <a name="building-managed-c-cppcli-for-nano-server"></a>Compilazione di C++ gestito (CPP/CLI) per Nano Server  
C++ gestito non è supportato da CoreCLR. Durante la conversione in CoreCLR, è quindi necessario riscrivere il codice C++ gestito in C# ed effettuare tutte le chiamate native tramite PInvoke.  
  
## <a name="porting-powershell-script-cmdlets"></a>Conversione dei cmdlet di script di PowerShell  
  
PowerShell Core offre una totale parità di linguaggio di PowerShell con altre edizioni di PowerShell, tra cui l'edizione in esecuzione su Windows 10 e Windows Server 2016. Quando si convertono cmdlet di script di PowerShell in Nano Server, tuttavia, tenere presenti le seguenti considerazioni:  
* Sono presenti dipendenze da altri cmdlet? Se sì, questi cmdlet sono disponibili in Nano Server? Vedere [PowerShell in Nano Server](PowerShell-on-Nano-Server.md) per informazioni sugli elementi non disponibili.  
* Se sono presenti dipendenze da assembly caricati in fase di runtime, gli script continueranno comunque a funzionare?   
* Come è possibile eseguire il debug dello script in remoto?   
* Come è possibile eseguire la migrazione da WMI .Net a MI .Net?  
  
### <a name="dependency-on-built-in-cmdlets"></a>Dipendenza da cmdlet integrati  
Non tutti i cmdlet di Windows Server 2016 sono disponibili in Nano Server (vedere [PowerShell in Nano Server](PowerShell-on-Nano-Server.md)). L'approccio migliore consiste nella configurazione di una macchina Nano Server virtuale in modo da poter verificare che siano disponibili i cmdlet necessari. A tale scopo, eseguire `Enter-PSSession` per connettersi al computer Nano Server di destinazione e quindi eseguire `Get-Command -CommandType Cmdlet, Function` per ottenere l'elenco dei cmdlet disponibili.  
  
### <a name="consider-using-powershell-classes"></a>Possibilità di usare le classi di PowerShell  
Per la compilazione di codice C# in linea in Nano Server è supportato il cmdlet Add-Type. Se si sta scrivendo nuovo codice o si sta convertendo codice esistente, è anche possibile valutare l'opportunità di usare classi di PowerShell per definire tipi personalizzati. È possibile usare classi di PowerShell sia per scenari di contenitori di proprietà sia per enumerazioni. Se è necessario eseguire un PInvoke, effettuare questa operazione usando C# tramite il cmdlet Add-Type o in un assembly precompilato.  
Ecco un esempio che illustra l'uso di Add-Type:  
  
```  
Add-Type -ReferencedAssemblies ([Microsoft.Management.Infrastructure.Ciminstance].Assembly.Location) -TypeDefinition @'  
public class TestNetConnectionResult  
{  
   // The compute name  
   public string ComputerName = null;  
   // The Remote IP address used for connectivity  
   public System.Net.IPAddress RemoteAddress = null;  
}  
'@  
# Create object and set properties  
$result = New-Object TestNetConnectionResult  
$result.ComputerName = "Foo"  
$result.RemoteAddress = 1.1.1.1  
  
```  
 Di seguito è riportato un uso di esempio di classi di PowerShell in Nano Server:  
   
```  
class TestNetConnectionResult    
{    
   # The compute name  
  [string] $ComputerName    
  
  #The Remote IP address used for connectivity    
  [System.Net.IPAddress] $RemoteAddress  
}  
# Create object and set properties  
$result = [TestNetConnectionResult]::new()  
$result.ComputerName = "Foo"  
$result.RemoteAddress = 1.1.1.1  
  
```  
  
### <a name="remotely-debugging-scripts"></a>Debug di script in remoto  
  
Per eseguire il debug di uno script in remoto, connettersi a un computer remoto specificando `Enter-PSsession` in un'istanza di PowerShell ISE. Una volta nella sessione, eseguire `psedit <file_path>` in modo che una copia del file venga aperta nell'istanza di PowerShell ISE locale. A questo punto è possibile eseguire il debug dello script come se fosse eseguito in locale impostando punti di interruzione. Eventuali modifiche introdotte in questo file verranno salvate anche nella versione remota.   
  
### <a name="migrating-from-wmi-net-to-mi-net"></a>Migrazione da WMI .NET a MI .NET  
  
Non essendo fornito il supporto per [WMI .NET](https://msdn.microsoft.com/library/mt481551(v=vs.110).aspx), è necessario eseguire la migrazione di tutti i cmdlet basati sull'API di precedente generazione all'API WMI supportata: [MI. NET](https://msdn.microsoft.com/library/dn387184(v=vs.85).aspx). È possibile accedere a MI .NET direttamente tramite C# o attraverso i cmdlet del modulo CimCmdlets.   
  
### <a name="cimcmdlets-module"></a>Modulo CimCmdlets  
  
I cmdlet WMI v1 (ad esempio, Get-WmiObject) non sono supportati in Nano Server, ma sono supportati i cmdlet CIM del modulo CimCmdlets (ad esempio, Get-CimInstance). I cmdlet CIM corrispondono quasi perfettamente ai cmdlet WMI v1. Get-WmiObject, ad esempio, può essere correlato a Get-CimInstance per l'uso di parametri molto simili. La sintassi di chiamata del metodo è leggermente diversa, ma è ben documentata tramite Invoke-CimMethod. Prestare attenzione quando si immettono i parametri: MI .NET presenta requisiti più severi in termini di tipi di parametri del metodo.  
  
### <a name="c-api"></a>API C#  
  
WMI .NET include l'interfaccia WMIv1, mentre MI .NET include l'interfaccia WMIv2 (CIM). Le classi esposte possono essere diverse ma le operazioni sottostanti sono molto simili. Per eseguire attività, è possibile enumerare o ottenere istanze di oggetti e richiamare le operazioni su di essi.   
  
  


