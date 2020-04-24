---
title: Distribuire Nano Server
description: Illustra la creazione e la distribuzione di immagini, pacchetti, driver, domini, ruoli e funzionalità personalizzati
ms.prod: windows-server
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.topic: get-started-article
ms.assetid: 9f109c91-7c2e-4065-856c-ce9e2e9ce558
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 9eceb92c239ce222f9f1498dfdeb8a21220af86f
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80827114"
---
# <a name="deploy-nano-server"></a>Distribuire Nano Server

>Si applica a: Windows Server 2016

> [!IMPORTANT]
> A partire da Windows Server versione 1709, Nano Server sarà disponibile solo come [immagine del sistema operativo di base del contenitore](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Per informazioni, vedi [Modifiche apportate a Nano Server](nano-in-semi-annual-channel.md). 

Questo argomento offre le informazioni necessarie per distribuire immagini di Nano Server più vicine alle proprie esigenze rispetto ai semplici esempi illustrati nell'argomento Avvio rapido di Nano Server. È possibile trovare informazioni su come creare un'immagine personalizzata di Nano Server contenente esattamente le funzionalità desiderate, installare immagini di Nano Server da un disco rigido virtuale o un'immagine WIM, modificare file, usare domini, gestire pacchetti con metodi diversi e compiere operazioni con i ruoli del server.

## <a name="nano-server-image-builder"></a>Nano Server Image Builder

Nano Server Image Builder è uno strumento che consente di creare un'immagine personalizzata di Nano Server e supporti USB di avvio con l'aiuto di un'interfaccia grafica. Sulla base di una serie di informazioni inserite dall'utente, genera script di PowerShell riusabili che consentono di automatizzare facilmente installazioni coerenti di Nano Server con Windows Server 2016, edizione Datacenter o Standard.

È possibile scaricare lo strumento dall'[Area download](https://www.microsoft.com/download/details.aspx?id=54065). 

Questo strumento richiede anche [Windows Assessment and Deployment Kit (ADK)](https://developer.microsoft.comwindows/hardware/windows-assessment-deployment-kit).


Nano Server Image Builder crea immagini personalizzate di Nano Server in formato disco rigido virtuale, VHDX o ISO ed è in grado di creare supporti USB di avvio per distribuire Nano Server o rilevare la configurazione hardware di un server. È anche in grado di:

- Accettare le condizioni di licenza 
- Creare file in formato disco rigido virtuale, VHDX o ISO
- Aggiungere ruoli del server
- Aggiungere driver di dispositivo
- Impostare il nome computer, la password amministratore, il percorso del file di log e il fuso orario
- Configurare l'aggiunta a un dominio usando un account Active Directory esistente o un BLOB di aggiunta al dominio raccolto
- Abilitare il servizio WinRM per le comunicazioni all'esterno della subnet locale
- Abilitare ID di LAN virtuali e configurare indirizzi IP statici
- Inserire nuovi pacchetti di manutenzione in tempo reale
- Aggiungere uno script setupcomplete.cmd o altri script personalizzati da eseguire dopo l'elaborazione di unattend.xml
- Abilitare Servizi di gestione emergenze per l'accesso dalla console della porta seriale
- Abilitare servizi di sviluppo per abilitare driver firmati e applicazioni non firmate di test, la shell predefinita di PowerShell
- Abilitare il debug tramite protocollo seriale, USB, TCP/IP o IEEE 1394
- Creare supporti USB usando WinPE per la partizione del server e l'installazione dell'immagine Nano
- Creare supporti USB usando WinPE per il rilevamento della configurazione hardware esistente di Nano Server e la documentazione sullo schermo e in un file di log di tutti i dettagli, tra cui schede di rete, indirizzi MAC e tipo di firmware (BIOS o UEFI). Il processo di rilevamento elenca anche tutti i volumi del sistema e i dispositivi che non hanno alcun driver incluso nel pacchetto di driver per Server Core.

Se non si ha familiarità con i concetti appena elencati, leggere la parte rimanente dell'argomento e gli altri argomenti relativi a Nano Server per prepararsi adeguatamente a immettere nello strumento tutte le informazioni necessarie.

## <a name="creating-a-custom-nano-server-image"></a><a name=BKMK_CreateImage></a>Creazione di un'immagine personalizzata di Nano Server  
In Windows Server 2016, Nano Server viene distribuito su un supporto fisico contenente una cartella **NanoServer** in cui sono disponibili un'immagine WIM e una sottocartella denominata **Packages**. Si tratta dei file di pacchetto necessari per aggiungere funzionalità e ruoli server all'immagine del disco rigido virtuale da cui si eseguirà l'avvio.  

Puoi trovare e installare questi pacchetti anche con il provider NanoServerPackage del modulo PackageManagement (OneGet) di PowerShell. Vedi la sezione Installazione di ruoli e funzionalità online in questo argomento.  

La tabella seguente mostra i ruoli e funzionalità disponibili in questa versione di Nano Server, insieme alle opzioni di Windows PowerShell che installeranno i pacchetti necessari. Alcuni pacchetti vengono installati direttamente con i rispettivi parametri di Windows PowerShell (ad esempio -Compute), mentre altri vengono installati passando al parametro -Package i nomi dei pacchetti, che puoi combinare in un elenco delimitato da virgole. Puoi elencare in modo dinamico i pacchetti disponibili tramite il cmdlet Get-NanoServerPackage.  


|                                                                             Ruolo o funzionalità                                                                             |                                                                                                                                                                                                          Opzione                                                                                                                                                                                                           |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                     Ruolo Hyper-V (incluso NetQoS)                                                                     |                                                                                                                                                                                                         -Compute                                                                                                                                                                                                          |
|                                                   Clustering di failover e altri componenti, elencati in dettaglio dopo questa tabella                                                   |                                                                                                                                                                                                        -Clustering                                                                                                                                                                                                        |
| Driver di base per un'ampia gamma di schede di rete e controller di archiviazione. Si tratta dello stesso set di driver incluso in un'installazione Server Core di Windows Server 2016. |                                                                                                                                                                                                        -OEMDrivers                                                                                                                                                                                                        |
|                                                Ruolo File server e altri componenti di archiviazione, elencati in dettaglio dopo questa tabella                                                 |                                                                                                                                                                                                         -Storage                                                                                                                                                                                                          |
|                                                          Windows Defender, incluso un file di firma predefinito                                                           |                                                                                                                                                                                                         -Defender                                                                                                                                                                                                         |
|                         Server d'inoltro inverso per la compatibilità delle applicazioni, ad esempio framework di applicazioni comuni come Ruby, Node.js e così via.                         |                                                                                                                                                                                                  Incluso ora per impostazione predefinita                                                                                                                                                                                                  |
|                                                                             Ruolo server DNS                                                                             |                                                                                                                                                                                         -Package Microsoft-NanoServer-DNS-Package                                                                                                                                                                                         |
|                                                              PowerShell Desired State Configuration (DSC)                                                               |                                                                                                                               -Package Microsoft-NanoServer-DSC-Package<br />**Nota:** Per informazioni dettagliate, vedere [Uso di DSC in Nano Server](https://msdn.microsoft.com/powershell/dsc/nanoDsc).                                                                                                                               |
|                                                                    Internet Information Server (IIS)                                                                    |                                                                                                                                       -Package Microsoft-NanoServer-IIS-Package<br />**Nota:** per informazioni dettagliate sull'uso di IIS, vedi [IIS in Nano Server](IIS-on-Nano-Server.md).                                                                                                                                        |
|                                                                   Supporto dell'host per i contenitori di Windows                                                                   |                                                                                                                                                                                                        -Containers                                                                                                                                                                                                        |
|                                                               Agente System Center Virtual Machine Manager                                                               | -Package Microsoft-NanoServer-SCVMM-Package<br />-Package Microsoft-NanoServer-SCVMM-Compute-Package<br />**Nota:** usa il pacchetto SCVMM Compute solo se esegui il monitoraggio di Hyper-V. Per le distribuzioni iperconvergenti in VMM, devi specificare anche il parametro -Storage. Per altre informazioni, vedi la [documentazione di VMM](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-compute-add-nano-hyper-v). |
|                                                                 Agente System Center Operations Manager                                                                  |                                                                                                                 Installato separatamente. Per altre informazioni, vedi la documentazione di System Center Operations Manager all'indirizzo https://technet.microsoft.com/system-center-docs/om/manage/install-agent-on-nano-server.                                                                                                                 |
|                                                                 Data Center Bridging (incluso DCBQoS)                                                                 |                                                                                                                                                                                         -Package Microsoft-NanoServer-DCB-Package                                                                                                                                                                                         |
|                                                                     Distribuzione in una macchina virtuale                                                                      |                                                                                                                                                                                        -Package Microsoft-NanoServer-Guest-Package                                                                                                                                                                                        |
|                                                                     Distribuzione in un computer fisico                                                                     |                                                                                                                                                                                        - Package Microsoft-NanoServer-Host-Package                                                                                                                                                                                        |
|     BitLocker, Trusted Platform Module (TPM), crittografia dei volumi, identificazione della piattaforma, provider di crittografia e altre funzionalità correlate all'avvio sicuro     |                                                                                                                                                                                    -Package Microsoft-NanoServer-SecureStartup-Package                                                                                                                                                                                    |
|                                                                    Supporto Hyper-V per macchine virtuali schermate                                                                     |                                                                                                                                         -Package Microsoft-NanoServer-ShieldedVM-Package<br />**Nota:** questo pacchetto è disponibile solo per l'edizione Datacenter di Nano Server.                                                                                                                                         |
|                                                             Agente Simple Network Management Protocol (SNMP)                                                             |                                   -Package Microsoft-NanoServer-SNMP-Agent-Package.cab<br />**Nota:** non incluso con il supporto di installazione di Windows Server 2016. Disponibile solo online. Per informazioni dettagliate, vedi [Installazione di ruoli e funzionalità online](https://technet.microsoft.com/windows-server-docs/get-started/deploy-nano-server#a-namebkmkonlineainstalling-roles-and-features-online).                                    |
|               Servizio IPHelper che fornisce connettività tunnel con tecnologie di transizione IPv6 (6to4, ISATAP, proxy delle porte e Teredo) e IP-HTTPS               |                                -Package Microsoft-NanoServer-IPHelper-Service-Package.cab<br />**Nota:** non incluso con il supporto di installazione di Windows Server 2016. Disponibile solo online. Per informazioni dettagliate, vedi [Installazione di ruoli e funzionalità online](https://technet.microsoft.com/windows-server-docs/get-started/deploy-nano-server#a-namebkmkonlineainstalling-roles-and-features-online).                                 |

> [!NOTE]  
> Quando si installa un pacchetto con queste opzioni, viene installato anche un Language Pack corrispondente in base alle impostazioni locali del supporto server selezionato. È possibile trovare i Language Pack disponibili e le rispettive abbreviazioni delle impostazioni locali nel supporto di installazione e, in particolare, nelle sottocartelle denominate in base alle impostazioni locali dell'immagine.  

> [!NOTE]  
> Se si installa la funzione Servizi file usando il parametro -Storage, questa funzione di fatto non viene abilitata. È possibile abilitarla da un computer remoto con Server Manager. 

### <a name="failover-clustering-items-installed-by-the--clustering-parameter"></a>Elementi del clustering di failover installati dal parametro -Clustering

- Ruolo Clustering di failover
- Clustering di failover delle macchine virtuali
- Spazi di archiviazione diretta (S2D)
- QoS di archiviazione
- Clustering di replica del volume
- Servizio Controllo SMB


### <a name="file-and-storage-items-installed-by-the--storage-parameter"></a>Elementi di file e archiviazione installati dal parametro -Storage

- Ruolo File server
- Deduplicazione dati
- Multipath I/O, incluso un driver per Microsoft Device-Specific Module (MSDSM)
- ReFS (v1 e v2)
- Iniziatore iSCSI (ma non destinazione iSCSI)
- Replica archiviazione
- Servizio di gestione dell'archiviazione con supporto SMI-S
- Servizio Controllo SMB
- Volumi dinamici
- Provider di archiviazione Windows di base (per la gestione dell'archiviazione di Windows)




### <a name="installing-a-nano-server-vhd"></a>Installazione di un disco rigido virtuale di Nano Server  
Nell'esempio seguente viene creata un'immagine VHDX basata su GPT con un determinato nome computer e dotata di driver guest Hyper-V, a partire da un supporto di installazione di Nano Server in una condivisione di rete. A un prompt di Windows PowerShell con privilegi elevati digitare il cmdlet seguente:  

`Import-Module <Server media location>\NanoServer\NanoServerImageGenerator; New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\server_en-us -BasePath .\Base -TargetPath .\FirstStepsNano.vhdx -ComputerName FirstStepsNano`  

Il cmdlet eseguirà tutte le attività seguenti:  

1.  Selezionare Standard come edizione di base  

2.  Richiedere la password amministratore  

3.  Copiare il supporto di installazione da \\\Path\To\Media\server_en-us into .\Base  

4.  Convertire l'immagine WIM in un disco rigido virtuale. (L'estensione di file dell'argomento del percorso di destinazione determina se viene creato un disco rigido virtuale basato su MBR per macchine virtuali di prima generazione o un VHDX basato su GPT per macchine virtuali di seconda generazione).  

5.  Copiare il disco rigido virtuale risultante in .\FirstStepsNano.vhdx  

6.  Impostare per l'immagine la password amministratore specificata  

7.  Impostare il nome computer dell'immagine su FirstStepsNano  

8.  Installare driver guest Hyper-V  

Al termine di questa procedura viene creata un'immagine di .\FirstStepsNano.vhdx.  

Mentre viene eseguito, il cmdlet genera anche un file di log di cui specificherà il percorso al termine dell'esecuzione. La conversione da WIM a disco rigido virtuale eseguita dallo script complementare genera un file di log distinto in %TEMP%\Convert-WindowsImage\\\<GUID> (dove \<GUID> è un identificatore univoco per ogni sessione di conversione).  

Se si usa lo stesso percorso di base, è possibile evitare di specificare il parametro relativo al percorso del supporto ogni volta che si esegue questo cmdlet, poiché vengono usati i file del percorso di base memorizzati nella cache. Se non si specifica un percorso di base, il cmdlet ne genererà uno predefinito nella cartella TEMP. Se si vuole usare supporti di origine diversi mantenendo lo stesso percorso di base, tuttavia, è necessario specificare ogni volta il parametro relativo al percorso del supporto.  

>[!NOTE]  
>È ora disponibile un'opzione che consente di specificare l'edizione di Nano Server desiderata: Standard o Datacenter. Usare il parametro -Edition per specificare l'edizione *Standard* o *Datacenter*.  

Si dispone ora di un'immagine esistente che è possibile modificare secondo le proprie necessità usando il cmdlet *Edit-NanoServerImage*.  

Se non si specifica un nome computer, verrà generato un nome casuale.  

### <a name="installing-a-nano-server-wim"></a>Installazione di un'immagine WIM di Nano Server  

1. Copiare la cartella *NanoServerImageGenerator* dalla cartella \NanoServer della ISO di Windows Server 2016 in una cartella locale sul computer.  
2. Avviare Windows PowerShell come amministratore, passare alla cartella in cui è stata posizionata la cartella NanoServerImageGenerator e quindi importare il modulo con `Import-Module .\NanoServerImageGenerator -Verbose`.  

   >[!NOTE]  
   >Può essere necessario modificare i criteri di esecuzione di Windows PowerShell. `Set-ExecutionPolicy RemoteSigned` dovrebbe funzionare correttamente.  

Per creare un'immagine di Nano Server che svolga la funzione di host Hyper-V, eseguire il cmdlet seguente:  

`New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerPhysical\NanoServer.wim -ComputerName <computer name> -OEMDrivers -Compute -Clustering`  

Dove  
-   -MediaPath è la radice del supporto DVD o dell'immagine ISO contenente Windows Server 2016.  
-   -BasePath conterrà una copia dei file binari di Nano Server, in modo che nelle esecuzione future sia possibile usare -BasePath di New-NanoServerImage senza dover specificare -MediaPath.  
-   -TargetPath conterrà il file risultante con estensione wim in cui sono inclusi i ruoli e le funzionalità selezionati. Accertarsi di specificare l'estensione wim.  
-   -Compute aggiunge il ruolo Hyper-V.  
-   -OemDrivers aggiunge alcuni driver comuni.  

Verrà richiesto di immettere una password amministratore.  

Per altre informazioni, eseguire `Get-Help New-NanoServerImage -Full`.  

Eseguire l'avvio in WinPE e verificare che il file con estensione wim appena creato sia accessibile da WinPE. (È possibile, ad esempio, copiare il file wim in un'immagine WinPE di avvio all'interno di un'unità flash USB.)  

All'avvio di WinPE usare Diskpart.exe per preparare il disco rigido del computer di destinazione. Eseguire i comandi Diskpart seguenti (apportare le necessarie modifiche se non si usano UEFI e GPT):  

> [!WARNING]  
> Questi comandi elimineranno tutti i dati presenti sul disco rigido.  

**Diskpart.exe Select disk 0 Clean Convert GPT Create partition efi size=100 Format quick FS=FAT32 label=System Assign letter=s Create partition msr size=128 Create partition primary Format quick FS=NTFS label=NanoServer Assign letter=n List volume Exit**  

Applicare l'immagine di Nano Server (modificare il percorso del file con estensione wim):  

**Dism.exe /apply-image /imagefile:.\NanoServer.wim /index:1 /applydir:n:\ Bcdboot.exe n:\Windows /s s:**  

Rimuovi il supporto DVD o l'unità USB e riavvia il sistema con il comando **Wpeutil.exe Reboot**  


### <a name="editing-files-on-nano-server-locally-and-remotely"></a>Modifica di file in Nano Server in locale o in remoto  
 In entrambi i casi, connettersi a Nano Server, ad esempio con la comunicazione remota di PowerShell.  

 Dopo essersi connessi a Nano Server è possibile modificare un file che risiede nel computer locale passando il percorso relativo o assoluto del file al comando psEdit, ad esempio:   
`psEdit C:\Windows\Logs\DISM\dism.log` o `psEdit .\myScript.ps1`  

Modificare un file che risiede in Nano Server remoto avviando una sessione remota con `Enter-PSSession -ComputerName 192.168.0.100 -Credential ~\Administrator` e quindi passando il percorso relativo o assoluto al comando psEdit, come descritto di seguito:   
`psEdit C:\Windows\Logs\DISM\dism.log`  

## <a name="installing-roles-and-features-online"></a><a name=BKMK_online></a>Installazione di ruoli e funzionalità online  
> [!NOTE]
> Se installi un pacchetto di Nano Server facoltativo da un supporto o un repository online, le correzioni recenti per la sicurezza non saranno incluse. Per evitare una mancata corrispondenza della versione tra i pacchetti facoltativi e il sistema operativo di base, devi installare l'[aggiornamento cumulativo più recente](https://technet.microsoft.com/windows-server-docs/get-started/update-nano-server) immediatamente dopo l'installazione di qualsiasi pacchetto facoltativo e **prima** del riavvio del server.

### <a name="installing-roles-and-features-from-a-package-repository"></a>Installazione di ruoli e funzionalità da un repository di pacchetti  
Puoi trovare e installare pacchetti di Nano Server dal repository di pacchetti online tramite il provider NanoServerPackage del modulo PackageManagement di PowerShell. Per installare il provider, usa questi cmdlet:

```powershell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
```

>[!NOTE]
>Se riscontri errori durante l'esecuzione di Install-PackageProvider, verifica di avere installato l'[aggiornamento cumulativo più recente](https://technet.microsoft.com/windows-server-docs/get-started/update-nano-server) ([KB3206632](https://support.microsoft.com/kb/3206632) o versione successiva) o usa Save-Module, come indicato di seguito: 

```powershell
Save-Module -Path $Env:ProgramFiles\WindowsPowerShell\Modules\ -Name NanoServerPackage -MinimumVersion 1.0.1.0
Import-PackageProvider NanoServerPackage
```

Dopo aver installato e importato il provider, puoi cercare, scaricare e installare pacchetti di Nano Server usando cmdlet appositamente progettati per questo scopo:

```powershell
Find-NanoServerPackage  
Save-NanoServerPackage  
Install-NanoServerPackage
```  

Puoi anche usare i cmdlet PackageManagement generici e specificare il provider NanoServerPackage:

```powershell
Find-Package -ProviderName NanoServerPackage
Save-Package -ProviderName NanoServerPackage
Install-Package -ProviderName NanoServerPackage
Get-Package -ProviderName NanoServerPackage
```

Per usare uno qualsiasi di questi cmdlet con i pacchetti di Nano Server in Nano Server, aggiungi `-ProviderName NanoServerPackage`. Se non aggiungi il parametro -ProviderName, PackageManagement eseguirà l'iterazione di tutti i provider. Per altri dettagli su questi cmdlet, esegui `Get-Help <cmdlet>`. Ecco alcuni esempi di utilizzo comuni.

### <a name="searching-for-nano-server-packages"></a>Ricerca di pacchetti di Nano Server  
Puoi usare `Find-NanoServerPackage` o `Find-Package -ProviderName NanoServerPackage` per cercare e ottenere un elenco di pacchetti di Nano Server disponibili nel repository online. Ad esempio, puoi ottenere l'elenco di tutti i pacchetti più recenti:

```powershell
Find-NanoServerPackage
```

Se esegui `Find-Package -ProviderName NanoServerPackage -DisplayCulture`, vengono visualizzate tutte le impostazioni cultura disponibili.

Se hai bisogno di una versione specifica di impostazioni locali, ad esempio Inglese (Stati Uniti), puoi usare `Find-NanoServerPackage -Culture en-us` o  
`Find-Package -ProviderName NanoServerPackage -Culture en-us` o `Find-Package -Culture en-us -DisplayCulture`.

Per trovare un pacchetto specifico in base al nome di pacchetto, usa il parametro -Name. Questo parametro accetta anche i caratteri jolly. Ad esempio, per trovare tutti i pacchetti il cui nome contiene VMM, usa `Find-NanoServerPackage -Name *VMM*` o `Find-Package -ProviderName NanoServerPackage -Name *VMM*`.

Puoi trovare una determinata versione con i parametri -RequiredVersion, -MinimumVersion e -MaximumVersion. Per trovare tutte le versioni disponibili, usa -AllVersions. In caso contrario, viene restituita solo la versione più recente. Ad esempio: `Find-NanoServerPackage -Name *VMM* -RequiredVersion 10.0.14393.0`. In alternativa, per tutte le versioni: `Find-Package -ProviderName NanoServerPackage -Name *VMM* -AllVersions`

### <a name="installing-nano-server-packages"></a>Installazione di pacchetti di Nano Server  
Puoi installare un pacchetto di Nano Server (inclusi i relativi pacchetti di dipendenza, se previsti) in Nano Server a livello locale o in un'immagine offline con `Install-NanoServerPackage` o `Install-Package -ProviderName NanoServerPackage`. Entrambi i parametri accettano input dalla pipeline.

Per installare la versione più recente di un pacchetto di Nano Server in un'istanza di Nano Server online, usa `Install-NanoServerPackage -Name Microsoft-NanoServer-Containers-Package` o `Install-Package -Name Microsoft-NanoServer-Containers-Package`. PackageManagement userà le impostazioni cultura di Nano Server.

Puoi installare un pacchetto di Nano Server in un'immagine offline specificando una determinata versione e specifiche impostazioni cultura, come descritto di seguito:

`Install-NanoServerPackage -Name Microsoft-NanoServer-DCB-Package -Culture de-de -RequiredVersion 10.0.14393.0 -ToVhd C:\MyNanoVhd.vhd`

oppure:

`Install-Package -Name Microsoft-NanoServer-DCB-Package -Culture de-de -RequiredVersion 10.0.14393.0 -ToVhd C:\MyNanoVhd.vhd`

Di seguito sono riportati alcuni esempi di canalizzazione dei risultati della ricerca di pacchetti nel cmdlet di installazione:  

`Find-NanoServerPackage *dcb* | Install-NanoServerPackage` trova tutti i pacchetti il cui nome contiene dcb e li installa.

`Find-Package *nanoserver-compute-* | Install-Package` trova i pacchetti il cui nome contiene nanoserver-compute- e li installa.

`Find-NanoServerPackage -Name *nanoserver-compute* | Install-NanoServerPackage -ToVhd C:\MyNanoVhd.vhd` trova i pacchetti il cui nome contiene compute e li installa in un'immagine offline.

`Find-Package -ProviderName NanoserverPackage *nanoserver-compute-* | Install-Package -ToVhd C:\MyNanoVhd.vhd` esegue la stessa operazione con tutti i pacchetti il cui nome contiene nanoserver-compute-.

### <a name="downloading-nano-server-packages"></a>Download di pacchetti di Nano Server  

`Save-NanoServerPackage` o `Save-Package` permette di scaricare pacchetti e salvarli senza installarli. Entrambi i cmdlet accettano input dalla pipeline.

Ad esempio, per scaricare e salvare un pacchetto di Nano Server in una directory corrispondente al percorso con caratteri jolly, usa `Save-NanoServerPackage -Name Microsoft-NanoServer-DNS-Package -Path C:\`. In questo esempio non è stato specificato il parametro -Culture, quindi verranno usate le impostazioni cultura del computer locale. Non essendo stata specificata la versione, inoltre, verrà salvata la versione più recente.

`Save-Package -ProviderName NanoServerPackage -Name Microsoft-NanoServer-IIS-Package -Path C:\ -Culture it-IT -MinimumVersion 10.0.14393.0` salva una determinata versione per la lingua e le impostazioni locali italiane.

Puoi inviare i risultati della ricerca nella pipeline come mostrato in questi esempi:

`Find-NanoServerPackage -Name *containers* -MaximumVersion 10.2 -MinimumVersion 1.0 -Culture es-ES | Save-NanoServerPackage -Path C:\`

oppure

`Find-Package -ProviderName NanoServerPackage -Name *shield* -Culture es-ES | Save-Package -Path`

### <a name="inventory-installed-packages"></a>Fare l'inventario dei pacchetti installati
Con `Get-Package` puoi scoprire quali pacchetti di Nano Server sono installati. Per vedere quali pacchetti sono presenti in Nano Server, ad esempio, puoi usare `Get-Package -ProviderName NanoserverPackage`.

Per verificare quali pacchetti di Nano Server sono installati in un'immagine offline, esegui invece `Get-Package -ProviderName NanoserverPackage -FromVhd C:\MyNanoVhd.vhd`.


### <a name="installing-roles-and-features-from-local-source"></a>Installazione di ruoli e funzionalità da un'origine locale  
Anche se l'installazione offline dei ruoli server e di altri pacchetti è l'opzione consigliata, può essere necessario installarli online, con Nano Server in esecuzione, in scenari di contenitore. A tale scopo, effettuare le operazioni seguenti:  

1.  Copiare la cartella Packages dai supporti di installazione nell'istanza di Nano Server in esecuzione (ad esempio in C:\packages).  

2.  Creare un nuovo file Unattend.xml su un altro computer e copiarlo quindi in Nano Server. È possibile copiare e incollare il contenuto XML nel file XML creato (in questo esempio viene illustrata l'installazione del pacchetto IIS):  



```  
<?xml version=1.0 encoding=utf-8?>
    <unattend xmlns=urn:schemas-microsoft-com:unattend>  
    <servicing>  
        <package action=install>  
            <assemblyIdentity name=Microsoft-NanoServer-IIS-Feature-Package version=10.0.14393.0 processorArchitecture=amd64 publicKeyToken=31bf3856ad364e35 language=neutral />  
            <source location=c:\packages\Microsoft-NanoServer-IIS-Package.cab />  
        </package>  
        <package action=install>  
            <assemblyIdentity name=Microsoft-NanoServer-IIS-Feature-Package version=10.0.14393.0 processorArchitecture=amd64 publicKeyToken=31bf3856ad364e35 language=en-US />  
            <source location=c:\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab />  
        </package>  
    </servicing>  
    <cpi:offlineImage cpi:source= xmlns:cpi=urn:schemas-microsoft-com:cpi />  
</unattend>  
```  




3. Nel nuovo file XML creato (o copiato), sostituire C:\packages con la directory in cui è stato copiato il contenuto dei pacchetti.  

4. Passare alla directory contenente il file XML appena creato ed eseguire quanto segue:  

   **dism /online /apply-unattend:.\unattend.xml**  



5. Confermare che il pacchetto e il Language Pack associato siano installati correttamente eseguendo quanto segue:  

   **dism /online /get-packages**  

   Package Identity: Microsoft-NanoServer-IIS-Package~31bf3856ad364e35~amd64~en-US~10.0.10586.0 sarà elencato due volte, una per Release Type: Language Pack e una per Release Type: Feature Pack.  

## <a name="customizing-an-existing-nano-server-vhd"></a>Personalizzazione di un disco rigido virtuale di Nano Server esistente  
È possibile modificare i dettagli di un disco rigido virtuale esistente usando il cmdlet Edit-NanoServerImage, come nell'esempio seguente:  

`Edit-NanoServerImage   -BasePath .\Base   -TargetPath .\BYOVHD.vhd`  

Questo cmdlet esegue le stesse operazioni di New-NanoServerImage ma, anziché convertire un'immagine WIM in un disco rigido virtuale, modifica l'immagine esistente. Supporta gli stessi parametri di New-NanoServerImage, ad eccezione di -MediaPath e -MaxSize, in modo da dover necessariamente creare il disco rigido virtuale iniziale con quei parametri prima di poter apportare modifiche con Edit-NanoServerImage.  

## <a name="additional-tasks-you-can-accomplish-with-new-nanoserverimage-and-edit-nanoserverimage"></a>Attività aggiuntive che è possibile eseguire con New-NanoServerImage e Edit-NanoServerImage  

### <a name="joining-domains"></a>Aggiunta di domini  
New-NanoServerImage offre due metodi di aggiunta a un dominio: entrambi si basano sul provisioning del dominio offline, ma uno raccoglie un BLOB per eseguire questa operazione. In questo esempio, il cmdlet raccoglie un BLOB di dominio per il dominio Contoso dal computer locale (che ovviamente deve far parte del dominio Contoso) e quindi esegue il provisioning offline dell'immagine usando il BLOB:  

`New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\JoinDomHarvest.vhdx -ComputerName JoinDomHarvest -DomainName Contoso`  

Al termine dell'esecuzione di questo cmdlet, dovresti trovare un computer denominato JoinDomHarvest nell'elenco dei computer di Active Directory.  

È possibile usare questo cmdlet anche in un computer non appartenente a un dominio. In questo caso, è necessario raccogliere un BLOB da un computer appartenente al dominio e quindi fornire il BLOB al cmdlet. Tenere presente che, quando si raccoglie un BLOB di questo tipo da un altro computer, il BLOB include già il nome computer. Pertanto, se si tenta di aggiungere il parametro *-ComputerName*, viene restituito un errore.  

È possibile raccogliere il BLOB con questo comando:  

**djoin**  
 **/Provision**  
 **/Domain Contoso**  
 **/Machine JoiningDomainsNoHarvest**  
 **/SaveFile JoiningDomainsNoHarvest.djoin**  

Eseguire New-NanoServerImage usando il BLOB raccolto:  

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\JoinDomNoHrvest.vhd -DomainBlobPath .\Path\To\Domain\Blob\JoinDomNoHrvestContoso.djoin`  

Nel caso in cui nel dominio sia già presente un nodo con lo stesso nome computer del futuro Nano Server, è possibile riusare il nome computer aggiungendo il parametro `-ReuseDomainNode`.  

### <a name="adding-additional-drivers"></a>Integrazione di driver aggiuntivi
Nano Server offre un pacchetto contenente un set di driver di base per un'ampia gamma di schede di rete e controller di archiviazione. È possibile tuttavia che non siano inclusi i driver necessari per le specifiche schede di rete in uso. Seguendo questa procedura è possibile trovare i driver necessari in un sistema funzionante, estrarli e aggiungerli all'immagine di Nano Server.

1. Installare Windows Server 2016 nel computer fisico in cui si eseguirà Nano Server.
2. Aprire Gestione dispositivi e identificare i dispositivi appartenenti alle categorie seguenti:
3. Schede di rete
4. Controller di archiviazione
5. Unità disco
6. Fare clic con il pulsante destro del mouse sul nome di ogni dispositivo identificato e scegliere **Proprietà**. Nella finestra di dialogo visualizzata fare clic sulla scheda **Driver** e quindi su **Dettagli driver**.
7. Prendere nota del nome file e del percorso del file di driver visualizzato. Si supponga, ad esempio, che il file di driver sia e1i63x64.sys, disponibile in C:\Windows\System32\Drivers.
8. A un prompt dei comandi, cercare il file di driver eseguendo la ricerca di tutte le istanze con dir e1i*.sys /s /b. In questo esempio, il file di driver è presente anche nel percorso C:\Windows\System32\DriverStore\FileRepository\net1ic64.inf_amd64_fafa7441408bbecd\e1i63x64.sys.
9. A un prompt dei comandi con privilegi elevati passa alla directory in cui si trova il disco rigido virtuale di Nano Server ed esegui questi comandi: **md mountdir**

    **dism\dism /Mount-Image /ImageFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir**

    **dism\dism /Add-Driver /image:.\mountdir /driver: C:\Windows\System32\DriverStore\FileRepository\net1ic64.inf_amd64_fafa7441408bbecd**

    **dism\dism /Unmount-Image /MountDir:.\MountDir /Commit**
10. Ripetere questi passaggi per ogni file di driver necessario.

> [!NOTE]  
> Nella cartella in cui sono contenuti i driver devono essere presenti sia i file SYS sia i file INF corrispondenti. Nano Server supporta solo driver a 64\-bit firmati. 

### <a name="injecting-drivers"></a>Inserimento di driver  
Nano Server offre un pacchetto contenente un set di driver di base per un'ampia gamma di schede di rete e controller di archiviazione. È possibile tuttavia che non siano inclusi i driver necessari per le specifiche schede di rete in uso. Per consentire a New-NanoServerImage di cercare nella directory i driver disponibili e inserirli nell'immagine di Nano Server, è possibile usare questa sintassi:  

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\InjectingDrivers.vhdx -DriverPath .\Extra\Drivers`  

> [!NOTE]
> Nella cartella in cui sono contenuti i driver devono essere presenti sia i file SYS sia i file INF corrispondenti. Inoltre, Nano Server supporta solo driver a 64 bit firmati.

Con il parametro -DriverPath puoi anche passare una matrice di percorsi di file INF di driver:

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\InjectingDrivers.vhdx -DriverPath .\Extra\Drivers\netcard64.inf`

### <a name="connecting-with-winrm"></a>Connessione con WinRM  
Per potersi connettere a un computer Nano Server usando Gestione remota Windows (WinRM), da un altro computer che non si trova nella stessa subnet, è necessario aprire la porta 5985 per il traffico TCP in ingresso nell'immagine di Nano Server. Usare il cmdlet seguente:  

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\ConnectingOverWinRM.vhd -EnableRemoteManagementPort`  

### <a name="setting-static-ip-addresses"></a>Impostazione di indirizzi IP statici  
Per configurare un'immagine di Nano Server in modo da usare indirizzi IP statici, trovare prima il nome o l'indice dell'interfaccia che si vuole modificare usando Get-NetAdapter, netsh o la Console di ripristino di emergenza di Nano Server. Usare quindi i parametri -Ipv6Address, -Ipv6Dns, -Ipv4Address, -Ipv4SubnetMask, -Ipv4Gateway e -Ipv4Dns per specificare la configurazione, come in questo esempio:  

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\StaticIpv4.vhd -InterfaceNameOrIndex Ethernet -Ipv4Address 192.168.1.2 -Ipv4SubnetMask 255.255.255.0 -Ipv4Gateway 192.168.1.1 -Ipv4Dns 192.168.1.1`  

### <a name="custom-image-size"></a>Dimensioni dell'immagine personalizzata  
È possibile configurare l'immagine di Nano Server in modo da essere costituita da un file VHDX o un disco rigido virtuale a espansione dinamica con il parametro -MaxSize, come in questo esempio:  

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\BigBoss.vhd -MaxSize 100GB`  

### <a name="embedding-custom-data"></a>Integrazione di dati personalizzati  
Per incorporare i file binari o script nell'immagine di Nano Server, usa il parametro -CopyPath per passare una matrice di file e directory da copiare. Il parametro -CopyPath può accettare anche una tabella hash per specificare il percorso di destinazione di file e directory.

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\BigBoss.vhd -CopyPath .\tools`

### <a name="running-custom-commands-after-the-first-boot"></a>Esecuzione di comandi personalizzati dopo il primo avvio
Per eseguire comandi personalizzati nell'ambito di setupcomplete.cmd, usa il parametro -SetupCompleteCommand per passare una matrice di comandi:

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -SetupCompleteCommand @(echo foo, echo bar)`


### <a name="running-custom-powershell-scripts-as-part-of-image-creation"></a>Esecuzione di script PowerShell personalizzati durante la creazione dell'immagine
Per eseguire script PowerShell personalizzati durante il processo di creazione dell'immagine, usa il parametro -OfflineScriptPath per passare una matrice di percorsi di script con estensione ps1. Se gli script accettano argomenti, usa -OfflineScriptArgument per passare una tabella hash di argomenti aggiuntivi agli script.

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -OfflineScriptPath C:\MyScripts\custom.ps1 -OfflineScriptArgument @{Param1=Value1; Param2=Value2}`


### <a name="support-for-development-scenarios"></a>Supporto per scenari di sviluppo
Se vuoi eseguire attività di sviluppo e test in Nano server, puoi usare il parametro -Development. Questo accorgimento consente infatti di abilitare PowerShell come shell locale predefinita, abilitare l'installazione di driver senza firma, copiare i file binari del debugger, aprire una porta per il debug, abilitare la firma di test e consentire l'installazione di pacchetti AppX senza una licenza per sviluppatori:

`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -Development`


### <a name="custom-unattend-file"></a>File di installazione automatica personalizzato  
Se si vuole usare un file di installazione automatica personalizzato, usare il parametro -UnattendPath:  

`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -UnattendPath \\path\to\unattend.xml`  

Se nel file di installazione automatica si specifica un nome computer o una password amministratore, i valori impostati da -AdministratorPassword e -ComputerName verranno sostituiti. 

> [!NOTE]
> Nano Server non supporta la configurazione di impostazioni TCP/IP tramite file di installazione automatica. Puoi usare Setupcomplete.cmd per configurare le impostazioni TCP/IP.

### <a name="collecting-log-files"></a>Raccolta di file di log
Se vuoi raccogliere i file di log durante la creazione dell'immagine, usa il parametro -LogPath per specificare la directory in cui copiare tutti i file di log.

`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -LogPath C:\Logs`


> [!NOTE]
> Alcuni parametri in New-NanoServerImage e Edit-NanoServerImage sono destinati al solo uso interno e possono essere ignorati senza problemi. Tra questi sono inclusi i parametri -SetupUI e -Internal.


## <a name="installing-apps-and-drivers"></a>Installazione di app e driver
[comment]: # (Da Xumin Sun, bug n. 68620).  

### <a name="windows-server-app-installer"></a>Programma di installazione di Windows Server App
Il programma di installazione di Windows Server App (WSA) offre un'opzione di installazione particolarmente affidabile per Nano Server. Poiché in Nano Server non è supportato Windows Installer (MSI), WSA è anche l'unica tecnologia di installazione disponibile per i prodotti non Microsoft. WSA si basa sulla tecnologia dei pacchetti app Windows progettata per installare e gestire le applicazioni in modo sicuro e affidabile tramite un manifesto dichiarativo. Estende il programma di installazione dei pacchetti app Windows in modo da supportare estensioni specifiche di Windows Server, con l'unica eccezione che WSA non supporta l'installazione di driver.

Per creare e installare un pacchetto WSA in Nano Server, sia l'editore che l'utente del pacchetto devono eseguire una serie di passaggi.

L'editore del pacchetto deve completare le operazioni seguenti:

1. Installa [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk), che contiene gli strumenti necessari per creare un pacchetto WSA: MakeAppx, MakeCert, Pvk2Pfx, SignTool.
2. Dichiara un manifesto: segui lo [schema di estensione per il manifesto WSA](https://msdn.microsoft.com/library/windows/apps/mt670653.aspx) per creare il file manifesto AppxManifest.xml.
3. Usare lo strumento **MakeAppx** per creare un pacchetto WSA.
4. Usare gli strumenti **MakeCert** e **Pvk2Pfx** per creare il certificato e quindi **Signtool** per firmare il pacchetto.

Successivamente, l'utente del pacchetto deve seguire questa procedura:

1. Esegui il cmdlet di PowerShell [*Import-Certificate*](https://technet.microsoft.com/library/hh848630) per importare in Nano Server il certificato dell'editore creato al precedente passaggio 4 con certStoreLocation in Cert:\LocalMachine\TrustedPeople. Ad esempio: `Import-Certificate -FilePath .\xyz.cer -CertStoreLocation Cert:\LocalMachine\TrustedPeople`
2. Installare l'app in Nano Server eseguendo il cmdlet di PowerShell [**Add-AppxPackage**](https://technet.microsoft.com/library/mt575516(v=wps.620).aspx) per installare un pacchetto WSA in Nano Server. Ad esempio: `Add-AppxPackage wsaSample.appx`

#### <a name="additional-resources-for-creating-apps"></a>Risorse aggiuntive per la creazione di app
WSA è l'estensione server della tecnologia dei pacchetti dell'app di Windows (anche se non è ospitata in Microsoft Store). Se vuoi pubblicare app con WSA, questi argomenti ti consentiranno di acquisire familiarità con la pipeline di creazione di pacchetti di app:

- [Come creare un manifesto di base del pacchetto](https://msdn.microsoft.com/library/windows/desktop/br211475.aspx)
- [Strumento per la creazione di pacchetti di app (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx)
- [Come creare un certificato di firma del pacchetto di un'app](https://msdn.microsoft.com/library/windows/desktop/jj835832(v=vs.85).aspx)
- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx)

### <a name="installing-drivers-on-nano-server"></a>Installazione di driver in Nano Server
In Nano Server è possibile installare driver non Microsoft usando pacchetti di driver INF, che includono sia pacchetti di driver Plug-and-Play (PnP) sia pacchetti di driver di filtro per il file system. In Nano Server non sono attualmente supportati driver di filtro di rete.

Sia i pacchetti di driver PnP sia quelli di filtro per il file system devono rispettare il processo di installazione e i requisiti dei driver universali, nonché le linee guida generali dei pacchetti di driver, come la firma. Questi aspetti sono documentati negli argomenti seguenti:

- [Firma di driver](https://msdn.microsoft.com/windows/hardware/drivers/install/driver-signing)
- [Uso di un file INF universale](https://msdn.microsoft.com/windows/hardware/drivers/install/using-a-configurable-inf-file)

#### <a name="installing-driver-packages-offline"></a>Installazione offline di pacchetti di driver

I pacchetti di driver supportati possono essere installati in Nano Server in modalità offline tramite i cmdlet [DISM.exe](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/dism-driver-servicing-command-line-options-s14) o [DISM di PowerShell](https://technet.microsoft.com/library/dn376497.aspx).

#### <a name="installing-driver-packages-online"></a>Installazione online di pacchetti di driver
I pacchetti di driver PnP possono essere installati in Nano Server in modalità online usando lo strumento [PnpUtil](https://msdn.microsoft.com/library/windows/hardware/ff550419(v=vs.85).aspx). Non è attualmente supportata in Nano Server l'installazione online di pacchetti di driver non PnP.






--------------------------------------------------  


## <a name="joining-nano-server-to-a-domain"></a><a name=BKMK_JoinDomain></a>Aggiunta di Nano Server a un dominio  

### <a name="to-add-nano-server-to-a-domain-online"></a>Per aggiungere Nano Server a un dominio in modalità online  

1.  Raccogliere un BLOB di dati da un computer del dominio che esegue già Windows Threshold Server usando il comando seguente:  

    `djoin.exe /provision /domain <domain-name> /machine <machine-name> /savefile .\odjblob`  

    Il BLOB di dati viene salvato in un file denominato odjblob.  

2.  Copia il file odjblob nel computer Nano Server con i comandi seguenti:  

    **net use z: \\\\\<indirizzo IP di Nano Server>\c$**  

    > [!NOTE]  
    > Se il comando net use non riesce , è probabile che sia necessario modificare le regole del firewall di Windows. A questo scopo, aprire un prompt dei comandi con privilegi elevati, avviare Windows PowerShell e quindi connettersi al computer Nano Server che esegue Comunicazione remota di Windows PowerShell con i comandi seguenti:  
    >   
    > `Set-Item WSMan:\localhost\Client\TrustedHosts <IP address of Nano Server>`  
    >   
    > `$ip = <ip address of Nano Server>`  
    >   
    > `Enter-PSSession -ComputerName $ip -Credential $ip\Administrator`  
    >   
    > Specificare la password amministratore, se richiesto, e quindi eseguire questo comando per impostare la regola del firewall appropriata:  
    >   
    > **netsh advfirewall firewall set rule group=Condivisione file e stampanti new enable=yes**  
    >   
    > Uscire da Windows PowerShell con `Exit-PSSession` e quindi ripetere il comando net use. Se ha esito positivo, continua a copiare il contenuto del file odjblob in Nano Server.  

    **md z:\Temp**  

    **copy odjblob z:\Temp**  

3.  Verificare che nel dominio a cui si vuole aggiungere Nano Server sia configurato il DNS. Verificare inoltre il corretto funzionamento della risoluzione dei nomi del dominio o di un controller del dominio. A questo scopo, aprire un prompt dei comandi con privilegi elevati, avviare Windows PowerShell e quindi connettersi al computer Nano Server che esegue Comunicazione remota di Windows PowerShell con i comandi seguenti:  

    `Set-Item WSMan:\localhost\Client\TrustedHosts <IP address of Nano Server>`  

    `$ip = <ip address of Nano Server>`  

    `Enter-PSSession -ComputerName $ip -Credential $ip\Administrator`  

    Quando richiesto, specificare la password amministratore. Nslookup non è disponibile in Nano Server ed è quindi necessario verificare la risoluzione dei nomi con Resolve-DNSName.

4. Se la risoluzione dei nomi ha esito positivo, nella stessa sessione di Windows PowerShell eseguire questo comando per completare l'aggiunta al dominio:  

    `djoin /requestodj /loadfile c:\Temp\odjblob /windowspath c:\windows /localos`  

5.  Riavviare il computer Nano Server e quindi uscire dalla sessione di Windows PowerShell:  

    `shutdown /r /t 5`  

    `Exit-PSSession`  

6.  Dopo aver aggiunto Nano Server a un dominio, aggiungere l'account utente del dominio al gruppo Administrators di Nano Server.

7. Per sicurezza, rimuovi il computer Nano Server dall'elenco degli host attendibili con questo comando: `Set-Item WSMan:\localhost\client\TrustedHosts ` 

**Metodo alternativo per l'aggiunta a un dominio in un solo passaggio**  

Raccogliere il BLOB di dati da un computer del dominio che esegue già Windows Threshold Server usando il comando seguente:  

`djoin.exe /provision /domain <domain-name> /machine <machine-name> /savefile .\odjblob`  

Apri il file odjblob (ad esempio nel Blocco note), copiane il contenuto e incollalo nella sezione \<AccountData> del file Unattend.xml seguente.  

Inserire il file Unattend.xml nella cartella C:\NanoServer e quindi usare i comandi seguenti per montare il disco rigido virtuale e applicare le impostazioni nella sezione `offlineServicing`:  

**dism\dism /Mount-ImagemediaFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir**  

**dism\dismmedia:.\mountdir /Apply-Unattend:.\unattend.xml**  

Crea una cartella Panther, che viene usata dai sistemi Windows per l'archiviazione dei file durante l'installazione. Vedi eventualmente [Percorsi di file registro di installazione di Windows 7, Windows Server 2008 R2 e Windows Vista](https://support.microsoft.com/kb/927521). Copia il file Unattend.xml nella cartella e quindi smonta il disco rigido virtuale con i comandi seguenti:  

**md .\mountdir\windows\panther**  

**copy .\unattend.xml .\mountdir\windows\panther**  

**dism\dism /Unmount-Image /MountDir:.\mountdir /Commit**  

Al primo avvio di Nano Server da questo disco rigido virtuale verranno automaticamente applicate le altre impostazioni.  

Dopo aver aggiunto Nano Server a un dominio, aggiungere l'account utente del dominio al gruppo Administrators di Nano Server.  

## <a name="working-with-server-roles-on-nano-server"></a>Uso dei ruoli server in Nano Server

### <a name="using-hyper-v-on-nano-server"></a>Uso di Hyper-V in Nano Server  
Hyper-V funziona in Nano Server come nella modalità Server Core di Windows Server, con due eccezioni:  

-   È necessario eseguire tutte le operazioni di gestione in remoto e il computer di gestione deve essere eseguito nella stessa build di Windows Server in cui si trova Nano Server. Non possono essere usate versioni precedenti dei cmdlet di Windows PowerShell per Hyper-V o per la console di gestione di Hyper-V.  

-   RemoteFX non è disponibile.  

In questa versione sono state confermate le funzionalità di Hyper-V seguenti:  

-   Abilitazione di Hyper-V  

-   Creazione di macchine virtuali di prima e di seconda generazione  

-   Creazione di commutatori virtuali  

-   Avvio di macchine virtuali ed esecuzione di sistemi operativi guest di Windows  
- Replica Hyper-V  



Se si vuole eseguire una migrazione in tempo reale di macchine virtuali, creare una macchina virtuale in una condivisione SMB oppure connettere le risorse di una condivisione SMB esistente a una macchina virtuale esistente, è di fondamentale importanza configurare correttamente l'autenticazione. Sono disponibili due opzioni per eseguire questa operazione:  

**Delega vincolata**  

La delega vincolata non ha subito variazioni rispetto alle versioni precedenti. Per altre informazioni, fare riferimento a questi articoli:  

-   [Abilitazione della gestione remota di Hyper-V: configurazione della delega vincolata per SMB e SMB a disponibilità elevata](https://blogs.msdn.com/b/taylorb/archive/2012/03/20/enabling-hyper-v-remote-management-configuring-constrained-delegation-for-smb-and-highly-available-smb.aspx)  

-   [Abilitazione della gestione remota di Hyper-V: configurazione della delega vincolata per la migrazione in tempo reale non in cluster](https://blogs.msdn.com/b/taylorb/archive/2012/03/20/enabling-hyper-v-remote-management-configuring-constrained-delegation-for-non-clustered-live-migration.aspx)  

**CredSSP**  

In primo luogo, fai riferimento alla sezione relativa all'uso della comunicazione remota di Windows PowerShell in questo argomento per attivare e testare CredSSP. Quindi, nel computer di gestione usa la console di gestione di Hyper-V e seleziona l'opzione Connetti come altro utente. La console di gestione di Hyper-V userà CredSSP. È necessario eseguire questa operazione anche se si sta usando l'account corrente.  

I cmdlet di Windows PowerShell per Hyper-V possono usare il parametro CimSession o Credential, poiché sono entrambi compatibili con CredSSP.  

### <a name="using-failover-clustering-on-nano-server"></a><a name=BKMK_Failover></a>Uso del clustering di failover in Nano Server  
Il clustering di failover funziona in Nano Server come nella modalità Server Core di Windows Server. È tuttavia necessario tenere sempre presente quanto segue:  

-   I cluster devono essere gestiti in remoto con Gestione cluster di failover o Windows PowerShell.  

-   Tutti i nodi cluster di Nano Server devono appartenere allo stesso dominio, come per i nodi cluster in Windows Server.  

-   L'account di dominio deve avere privilegi di amministratore su tutti i nodi di Nano Server, come per i nodi cluster in Windows Server.  

-   Tutti i comandi devono essere eseguiti a un prompt dei comandi con privilegi elevati.  

> [!NOTE]  
> In questa versione, inoltre, non sono supportate alcune funzionalità:  
>   
> -   Non è possibile eseguire i cmdlet per il clustering di failover in un Nano Server locale tramite Windows PowerShell.  
> -   Ruoli di clustering diversi da Hyper-V e File server.  

Per la gestione di cluster di failover risulteranno particolarmente utili i cmdlet di Windows PowerShell seguenti:  

Puoi creare un nuovo cluster con `New-Cluster -Name <clustername> -Node <comma-separated cluster node list>`  

Dopo aver definito un nuovo cluster, è opportuno eseguire `Set-StorageSetting -NewDiskPolicy OfflineShared` su tutti i nodi.  

Puoi aggiungere un altro nodo al cluster con `Add-ClusterNode -Name <comma-separated cluster node list>  -Cluster <clustername>`  

Puoi rimuovere un nodo dal cluster con `Remove-ClusterNode -Name <comma-separated cluster node list>  -Cluster <clustername>`  

Puoi creare un file server di scalabilità orizzontale con `Add-ClusterScaleoutFileServerRole -name <sofsname> -cluster <clustername>`  

È possibile trovare altri cmdlet per il clustering di failover in [Microsoft.FailoverClusters.PowerShell](https://technet.microsoft.com/library/ee461009.aspx).  

### <a name="using-dns-server-on-nano-server"></a><a name=BKMK_DNS></a>Uso del server DNS in Nano Server  
Per aggiungere in Nano Server il ruolo Server DNS, aggiungi Microsoft-NanoServer-DNS-Package all'immagine (vedi la sezione Creazione di un'immagine personalizzata di Nano Server in questo argomento). Mentre Nano Server è in esecuzione, connettersi all'immagine ed eseguire questo comando da una console di Windows PowerShell con privilegi elevati per abilitare la funzionalità desiderata:  

`Enable-WindowsOptionalFeature -Online -FeatureName DNS-Server-Full-Role`  

### <a name="using-iis-on-nano-server"></a><a name=BKMK_IIS></a>Uso di IIS in Nano Server  
Per la procedura da seguire per usare il ruolo Internet Information Services (IIS), vedere [IIS in Nano Server](IIS-on-Nano-Server.md). 

### <a name="using-mpio-on-nano-server"></a>Uso di MPIO in Nano Server
Per la procedura da seguire per usare MPIO, vedere [MPIO in Nano Server](MPIO-on-Nano-Server.md) 

### <a name="using-ssh-on-nano-server"></a><a name=BKMK_SSH></a>Uso di SSH in Nano Server
Per istruzioni su come installare e usare SSH in Nano Server con il progetto OpenSSH, vedere l'articolo [wiki Win32-OpenSSH](https://github.com/PowerShell/Win32-OpenSSH/wiki).

## <a name="appendix-sample-unattendxml-file-that-joins-nano-server-to-a-domain"></a>Appendice: Esempio di file Unattend.xml che aggiunge Nano Server a un dominio  

> [!NOTE]  
> Assicurati di eliminare lo spazio finale nel contenuto copiato da odjblob dopo averlo incollato nel file di installazione automatica.  

```  
<?xml version='1.0' encoding='utf-8'?>  
<unattend xmlns=urn:schemas-microsoft-com:unattend xmlns:wcm=https://schemas.microsoft.com/WMIConfig/2002/State xmlns:xsi=http://www.w3.org/2001/XMLSchema-instance>  

  <settings pass=offlineServicing>  
    <component name=Microsoft-Windows-UnattendedJoin processorArchitecture=amd64 publicKeyToken=31bf3856ad364e35 language=neutral versionScope=nonSxS>  
        <OfflineIdentification>                
           <Provisioning>    
             <AccountData> AAAAAAARUABLEABLEABAoAAAAAAAMABSUABLEABLEABAwAAAAAAAAABbMAAdYABc8ABYkABLAABbMAAEAAAAMAAA0ABY4ABZ8ABbIABa0AAcIABY4ABb8ABZUABAsAAAAAAAQAAZoABNUABOYABZYAANQABMoAAOEAAMIAAOkAANoAAMAAAXwAAJAAAAYAAA0ABY4ABZ8ABbIABa0AAcIABY4ABb8ABZUABLEAALMABLQABU0AATMABXAAAAAAAKdf/mhfXoAAUAAAQAAAAb8ABLQABbMABcMABb4ABc8ABAIAAAAAAb8ABLQABbMABcMABb4ABc8ABLQABb0ABZIAAGAAAAsAAR4ABTQABUAAAAAAACAAAQwABZMAAZcAAUgABVcAAegAARcABKkABVIAASwAAY4ABbcABW8ABQoAAT0ABN8AAO8ABekAAJMAAVkAAZUABckABXEABJUAAQ8AAJ4AAIsABZMABdoAAOsABIsABKkABQEABUEABIwABKoAAaAABXgABNwAAegAAAkAAAAABAMABLIABdIABc8ABY4AADAAAA4AAZ4ABbQABcAAAAAAACAAkKBW0ID8nJDWYAHnBAXE77j7BAEWEkl+lKB98XC2G0/9+Wd1DJQW4IYAkKBAADhAnKBWEwhiDAAAM2zzDCEAM6IAAAgAAAAAAAQAAAAAAAAAAAABwzzAAA  
</AccountData>  
           </Provisioning>    
         </OfflineIdentification>    
    </component>  
  </settings>  

  <settings pass=oobeSystem>  
    <component name=Microsoft-Windows-Shell-Setup processorArchitecture=amd64 publicKeyToken=31bf3856ad364e35 language=neutral versionScope=nonSxS>  
      <UserAccounts>  
        <AdministratorPassword>  
           <Value>Tuva</Value>  
           <PlainText>true</PlainText>  
        </AdministratorPassword>  
      </UserAccounts>  
      <TimeZone>Pacific Standard Time</TimeZone>  
    </component>  
  </settings>  

  <settings pass=specialize>  
    <component name=Microsoft-Windows-Shell-Setup processorArchitecture=amd64 publicKeyToken=31bf3856ad364e35 language=neutral versionScope=nonSxS>  
      <RegisteredOwner>My Team</RegisteredOwner>  
      <RegisteredOrganization>My Corporation</RegisteredOrganization>  
    </component>  
  </settings>  
</unattend>  
```  

