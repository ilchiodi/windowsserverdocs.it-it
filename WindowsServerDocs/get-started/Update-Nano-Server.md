---
title: Aggiornamento di Nano Server
description: ''
ms.prod: windows-server
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.topic: get-started-article
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 3f8bbe4dcf9161686367f7807d522b79bcf99e32
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826404"
---
# <a name="updating-nano-server"></a>Aggiornamento di Nano Server

> [!IMPORTANT]
> A partire da Windows Server, versione 1709, Nano Server sarà disponibile solo come [immagine del sistema operativo di base del contenitore](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Per informazioni, vedi [Modifiche apportate a Nano Server](nano-in-semi-annual-channel.md). 

Nano Server offre una serie di metodi di aggiornamento. Rispetto ad altre opzioni di installazione di Windows Server, Nano Server segue un modello di servizio più attivo, simile a quello di Windows 10. Le versioni rilasciate periodicamente sono note come **Current Branch for Business (CBB)** . Questo approccio favorisce i clienti che vogliono innovare più velocemente e progredire con una cadenza di rapidi cicli di sviluppo di tipo cloud. Altre informazioni su CBB sono disponibili nel [blog di Windows Server](https://blogs.technet.microsoft.com/windowsserver/2016/07/12/windows-server-2016-new-current-branch-for-business-servicing-option/).

**Tra una versione CBB e l'altra**, Nano Server viene aggiornato con un serie di *aggiornamenti cumulativi*. Ad esempio, il primo aggiornamento cumulativo per Nano Server è stato reso disponibile il 26 settembre 2016 come [KB4093120](https://support.microsoft.com/help/4093120/windows-10-update-kb4093120). Con questo aggiornamento cumulativo e quelli successivi, offriamo diverse opzioni per installare gli aggiornamenti in Nano Server. In questo articolo useremo l'aggiornamento KB3192366 come esempio per illustrare come ottenere gli aggiornamenti cumulativi e applicarli a Nano Server. Per altre informazioni sul modello di aggiornamento cumulativo, vedi il [blog di Microsoft Update](https://blogs.technet.microsoft.com/mu/2016/10/25/patching-with-windows-server-2016/).

> [!NOTE]
> Se installi un pacchetto di Nano Server facoltativo da un supporto o un repository online, le correzioni recenti per la sicurezza non saranno incluse. Per evitare una mancata corrispondenza della versione tra i pacchetti facoltativi e il sistema operativo di base, è necessario installare l'aggiornamento cumulativo più recente immediatamente dopo l'installazione di un pacchetto facoltativo e **prima** del riavvio del server.

Nel caso dell'aggiornamento cumulativo per Windows Server 2016 del 26 settembre 2016 ([KB3192366](https://support.microsoft.com/kb/3192366)), è necessario installare prima il più recente aggiornamento dello stack di manutenzione per Windows 10 versione 1607 del 23 agosto 2016 come prerequisito ([KB3176936](https://support.microsoft.com/kb/3176936)). Per la maggior parte delle opzioni riportate di seguito, devi usare i file con estensione msu contenenti i pacchetti di aggiornamento CAB. Visita Microsoft Update Catalog per scaricare ognuno di questi pacchetti di aggiornamento:
- [https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3192366](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3192366)
- [https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3176936](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3176936)

Dopo aver scaricato i file con estensione msu da Microsoft Update Catalog, salvali in una condivisione di rete o una directory locale, ad esempio C:\ServicingPackages. Puoi rinominare i file con estensione msu in base al numero KB, come abbiamo fatto di seguito per poterli identificare più facilmente. Usa quindi l'utilità EXPAND per estrarre in directory separate i file CAB dai file con estensione msu e copia i file CAB in un'unica cartella.

```code
    mkdir C:\ServicingPackages_expanded
    mkdir C:\ServicingPackages_expanded\KB3176936
    mkdir C:\ServicingPackages_expanded\KB3192366
    Expand C:\ServicingPackages\KB3176936.msu -F:* C:\ServicingPackages_expanded\KB3176936
    Expand C:\ServicingPackages\KB3192366.msu -F:* C:\ServicingPackages_expanded\KB3192366
    mkdir C:\ServicingPackages_cabs
    copy C:\ServicingPackages_expanded\KB3176936\Windows10.0-KB3176936-x64.cab C:\ServicingPackages_cabs
    copy C:\ServicingPackages_expanded\KB3192366\Windows10.0-KB3192366-x64.cab C:\ServicingPackages_cabs
```

Ora puoi usare i file CAB estratti per applicare gli aggiornamenti a un'immagine di Nano Server in diversi modi, a seconda delle tue esigenze. Le opzioni seguenti sono presentate senza un ordine di preferenza particolare. Usa l'opzione più adatta per il tuo ambiente.

> [!NOTE]
> Quando usi gli strumenti di Gestione e manutenzione immagini distribuzione per la manutenzione di Nano Server, devi usare una versione corrispondente alla versione di Nano Server di cui esegui la manutenzione o più recente di quest'ultima. A questo scopo, puoi eseguire Gestione e manutenzione immagini distribuzione da una versione corrispondente di Windows, installare una versione corrispondente di [Windows Assessment and Deployment Kit (ADK)](https://developer.microsoft.comwindows/hardware/windows-assessment-deployment-kit) o eseguire Gestione e manutenzione immagini distribuzione in Nano Server stesso.

## <a name="option-1-integrate-a-cumulative-update-into-a-new-image"></a>Opzione 1: integrare un aggiornamento cumulativo in una nuova immagine
Se crei una nuova immagine di Nano Server, puoi integrare l'aggiornamento cumulativo più recente direttamente nell'immagine in modo che vengano applicate tutte le patch necessarie al primo avvio.

```powershell
New-NanoServerImage -ServicingPackagePath 'C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab', 'C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab' -<other parameters>
```

## <a name="option-2-integrate-a-cumulative-update-into-an-existing-image"></a>Opzione 2: integrare un aggiornamento cumulativo in un'immagine esistente
Se hai un'immagine di Nano Server che usi come base per la creazione di istanze specifiche di Nano Server, puoi integrare l'aggiornamento cumulativo più recente direttamente nell'immagine di base esistente, in modo che alle macchine create con l'immagine vengano applicate tutte le patch necessarie al primo avvio.

```powershell
Edit-NanoServerImage -ServicingPackagePath 'C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab', 'C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab' -TargetPath .\NanoServer.wim
```

## <a name="option-3-apply-the-cumulative-update-to-an-existing-offline-vhd-or-vhdx"></a>Opzione 3: applicare l'aggiornamento cumulativo a un disco VHD o VHDX offline esistente
Se hai un disco rigido virtuale (VHD o VHDX), puoi usare gli strumenti di Gestione e manutenzione immagini distribuzione per applicare l'aggiornamento a tale disco. Devi assicurarti che il disco non sia in uso arrestando le macchine virtuali che lo usano oppure smontando il file del disco rigido virtuale.

- Mediante PowerShell

   ```powershell
   Mount-WindowsImage -ImagePath .\NanoServer.vhdx -Path .\MountDir -Index 1
   Add-WindowsPackage -Path .\MountDir -PackagePath  C:\ServicingPackages_cabs
   Dismount-WindowsImage -Path .\MountDir -Save
   ```

- Uso di dism.exe

   ```code
   dism.exe /Mount-Image /ImageFile:C:\NanoServer.vhdx /Index:1 /MountDir:C:\MountDir
   dism.exe /Image:C:\MountDir /Add-Package /PackagePath:C:\ServicingPackages_cabs
   dism.exe /Unmount-Image /MountDir:C:\MountDir /Commit
   ```

## <a name="option-4-apply-the-cumulative-update-to-a-running-nano-server"></a>Opzione 4: applicare l'aggiornamento cumulativo a un'istanza di Nano Server in esecuzione
Se hai un host fisico o una macchina virtuale Nano Server in esecuzione e hai scaricato il file CAB per l'aggiornamento, puoi usare gli strumenti di Gestione e manutenzione immagini distribuzione per applicare l'aggiornamento mentre il sistema operativo è online. Dovrai copiare il file CAB in locale in Nano Server o in un percorso di rete accessibile. Se applichi un aggiornamento dello stack di manutenzione, assicurati di riavviare il server dopo aver applicato l'aggiornamento e prima di applicarne altri.

> [!NOTE]
> Se hai creato l'immagine di VHD o VHDX Nano Server usando il cmdlet New-NanoServerImage e non hai specificato MaxSize per il file del disco rigido virtuale, la dimensione predefinita di 4 GB è insufficiente per applicare l'aggiornamento cumulativo. Prima di installare l'aggiornamento, usa la console di gestione di Hyper-V, Gestione disco, PowerShell o un altro strumento per espandere le dimensioni del disco rigido virtuale e del volume di sistema ad almeno 10 GB oppure usa il parametro ScratchDir negli strumenti di Gestione e manutenzione immagini distribuzione per impostare la directory dei file temporanei su un volume con almeno 10 GB di spazio libero.

```powershell
$s = New-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
Copy-Item -ToSession $s -Path C:\ServicingPackages_cabs -Destination C:\ServicingPackages_cabs -Recurse
Enter-PSSession $s
```

- Mediante PowerShell

   ```powershell
   # Apply the servicing stack update first and then restart
   Add-WindowsPackage -Online -PackagePath C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab
   Restart-Computer; exit

   # After restarting, apply the cumulative update and then restart
   Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
   Add-WindowsPackage -Online -PackagePath C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab
   Restart-Computer; exit
   ```

- Uso di dism.exe
   ```powershell
   # Apply the servicing stack update first and then restart
   dism.exe /Online /Add-Package /PackagePath:C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab
   
   # After the operation completes successfully and you are prompted to restart, it's safe to
   # press Ctrl+C to cancel the pipeline and return to the prompt
   Restart-Computer; exit

   # After restarting, apply the cumulative update and then restart
   Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
   dism.exe /Online /Add-Package /PackagePath:C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab
   Restart-Computer; exit
   ```

## <a name="option-5-download-and-install-the-cumulative-update-to-a-running-nano-server"></a>Opzione 5: scaricare e installare l'aggiornamento cumulativo in un'istanza di Nano Server in esecuzione

Se hai un host fisico o una macchina virtuale Nano Server in esecuzione, puoi usare il provider WMI di Windows Update per scaricare e installare l'aggiornamento mentre il sistema operativo è online. Con questo metodo non devi scaricare il file con estensione msu separatamente da Microsoft Update Catalog. Il provider WMI rileva, scarica e installa tutti gli aggiornamenti disponibili contemporaneamente.

```powershell
Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
```

- Rilevare gli aggiornamenti disponibili
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  
   $result = $ci | Invoke-CimMethod -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=0";OnlineScan=$true}
   $result.Updates
   ```

- Installare tutti gli aggiornamenti disponibili
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession
   Invoke-CimMethod -InputObject $ci -MethodName ApplyApplicableUpdates
   Restart-Computer; exit
   ```

- Ottenere un elenco degli aggiornamenti installati
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession
   $result = $ci | Invoke-CimMethod -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=1";OnlineScan=$true}
   $result.Updates
   ```
   
## <a name="additional-options"></a>Opzioni aggiuntive
Altri metodi per l'aggiornamento di Nano Server possono sovrapporsi o integrarsi con le opzioni illustrate sopra. Tali opzioni includono l'uso di Windows Server Update Services (WSUS), di System Center Virtual Machine Manager (VMM), dell'Utilità di pianificazione o di una soluzione non Microsoft.
- [Configurazione di Windows Update per WSUS](https://msdn.microsoft.com/library/dd939844(v=ws.10).aspx) impostando le seguenti chiavi del Registro di sistema:
  - WUServer
  - WUStatusServer (in genere usa lo stesso valore di WUServer)
  - UseWUServer
  - AUOptions
- [Gestione degli aggiornamenti dell'infrastruttura in VMM](https://technet.microsoft.com/library/gg675084(v=sc.12).aspx)
- [Registrazione di un'attività pianificata](https://technet.microsoft.com/library/jj649811.aspx)