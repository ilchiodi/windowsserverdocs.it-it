---
ms.date: 01/07/2019
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, installare, configurare
contributor: maertendMSFT
author: maertendMSFT
title: Installazione di OpenSSH per Windows
ms.openlocfilehash: f617b01ee7dabd4897f99e374420f673e209e145
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859562"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>Installazione di OpenSSH per Windows 10 e Windows Server 2019 #

Il OpenSSH Client e OpenSSH Server sono componenti installabili separatamente nelle finestre 1809 10 e Windows Server 2019.
Gli utenti con queste versioni di Windows devono usare le istruzioni seguenti per installare e configurare OpenSSH. 

> [!NOTE] 
> Gli utenti che hanno acquisito OpenSSH dal repository Github di PowerShell (https://github.com/PowerShell/OpenSSH-Portable) devono usare le istruzioni da qui, e __non deve__ usare queste istruzioni. 


## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>Installazione di OpenSSH dalle impostazioni dell'interfaccia utente in Windows Server 2019 o Windows 10 1809

Server e client OpenSSH sono installabili funzionalità di Windows 10 1809. 

Per installare OpenSSH, impostazioni di avvio, quindi passare ad App > App e funzionalità > Gestisci funzionalità facoltative. 

Analizzare l'elenco per verificare se è già installato il client OpenSSH. In caso contrario, quindi nella parte superiore della pagina selezionare "Aggiungi una funzionalità", quindi: 

* Per installare il client OpenSSH, individuare "OpenSSH Client", quindi fare clic su "Installa". 
* Per installare il server di OpenSSH, individuare "OpenSSH Server", quindi fare clic su "Installa". 

Al termine dell'installazione, tornare alle App > App e funzionalità > Gestisci funzionalità facoltative che dovrebbe mostrare i componenti di OpenSSH elencati.

> [!NOTE]
> Installazione di OpenSSH Server verranno creare e abilitare una regola firewall denominata "OpenSSH-Server-In-TCP". In questo modo il traffico SSH in ingresso sulla porta 22. 

## <a name="installing-openssh-with-powershell"></a>Installazione di OpenSSH con PowerShell 

Per installare OpenSSH tramite PowerShell, avviare PowerShell come amministratore.
Per assicurarsi che le funzionalità di OpenSSH sono disponibili per l'installazione:

```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'

# This should return the following output:

Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent
Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

Quindi, installare le funzionalità server e/o client:

```powershell
# Install the OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Install the OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0

# Both of these should return the following output:

Path          :
Online        : True
RestartNeeded : False
```

## <a name="uninstalling-openssh"></a>Disinstallazione di OpenSSH

Per disinstallare OpenSSH utilizzando le impostazioni di Windows, impostazioni di avvio, quindi passare ad App > App e funzionalità > Gestisci funzionalità facoltative. Nell'elenco delle funzionalità installate, selezionare il componente OpenSSH Client o OpenSSH Server, quindi selezionare Disinstalla.

Per disinstallare OpenSSH tramite PowerShell, usare uno dei seguenti comandi:

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

Dopo la rimozione di OpenSSH, se il servizio è in uso al momento, è stata disinstallata, potrebbe essere necessario un riavvio di Windows.


## <a name="initial-configuration-of-ssh-server"></a>Configurazione iniziale del Server SSH

Per configurare il server OpenSSH per l'utilizzo iniziale su Windows, avviare PowerShell come amministratore, quindi eseguire i comandi seguenti per avviare il servizio SSHD:

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup. 
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled 
```

## <a name="initial-use-of-ssh"></a>Utilizzo iniziale di SSH

Dopo aver installato il OpenSSH Server in Windows, è possibile testare rapidamente usando PowerShell da qualsiasi dispositivo Windows con il Client SSH installato. In PowerShell digitare il comando seguente: 

```powershell
Ssh username@servername
```

La prima connessione a qualsiasi server genererà un messaggio simile al seguente:

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

La risposta deve essere "yes" o "no". Scegliendo Sì aggiungerà tale server al sistema locale dell'elenco di noto ssh host.

Richiederà la password a questo punto. Come misura di sicurezza, la password non essere visualizzata durante la digitazione. 

Dopo la connessione verrà visualizzato un prompt della shell comandi simile al seguente:

```
domain\username@SERVERNAME C:\Users\username>
```

La shell predefinita usata da OpenSSH di Windows server è la shell dei comandi di Windows. 

