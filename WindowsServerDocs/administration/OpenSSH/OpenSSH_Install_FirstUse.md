---
ms.date: 01/07/2019
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, install, Setup
contributor: maertendMSFT
author: maertendMSFT
title: Installazione di OpenSSH per Windows
ms.openlocfilehash: 6a5d4d47fbb3f962c2a19582eb0a72810145a28c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866881"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>Installazione di OpenSSH per Windows Server 2019 e Windows 10 #

Il client OpenSSH e il server OpenSSH sono componenti installabili separatamente in Windows Server 2019 e Windows 10 1809.
Gli utenti con queste versioni di Windows devono usare le istruzioni seguenti per installare e configurare OpenSSH. 

> [!NOTE] 
> Utenti che hanno acquisito OpenSSH dal repository GitHub di PowerShell https://github.com/PowerShell/OpenSSH-Portable) (usare le istruzioni da questa posizione e __non devono__ usare queste istruzioni. 


## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>Installazione di OpenSSH dall'interfaccia utente delle impostazioni in Windows Server 2019 o Windows 10 1809

Il client e il server OpenSSH sono funzionalità installabili di Windows 10 1809. 

Per installare OpenSSH, avviare impostazioni, quindi passare a App > app e funzionalità > Gestisci funzionalità facoltative. 

Analizza questo elenco per verificare se il client OpenSSH è già installato. In caso contrario, nella parte superiore della pagina selezionare "Aggiungi una funzionalità", quindi: 

* Per installare il client OpenSSH, individuare "client OpenSSH", quindi fare clic su "Installa". 
* Per installare il server OpenSSH, individuare "server OpenSSH", quindi fare clic su "Installa". 

Al termine dell'installazione, tornare alle app > app e funzionalità > gestire le funzionalità facoltative. si noterà che i componenti di OpenSSH sono elencati.

> [!NOTE]
> Se si installa il server OpenSSH, viene creata e abilitata una regola del firewall denominata "OpenSSH-server-in-TCP". Questo consente il traffico SSH in ingresso sulla porta 22. 

## <a name="installing-openssh-with-powershell"></a>Installazione di OpenSSH con PowerShell 

Per installare OpenSSH con PowerShell, avviare innanzitutto PowerShell come amministratore.
Per assicurarsi che le funzionalità di OpenSSH siano disponibili per l'installazione:

```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'

# This should return the following output:

Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent
Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

Installare quindi le funzionalità del server e/o del client:

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

Per disinstallare OpenSSH usando le impostazioni di Windows, avviare impostazioni, quindi passare a App > app e funzionalità > gestire le funzionalità facoltative. Nell'elenco delle funzionalità installate selezionare il componente OpenSSH client o OpenSSH server, quindi selezionare Disinstalla.

Per disinstallare OpenSSH usando PowerShell, usare uno dei comandi seguenti:

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

Dopo la rimozione di OpenSSH, potrebbe essere necessario riavviare Windows se il servizio è in uso al momento della disinstallazione.


## <a name="initial-configuration-of-ssh-server"></a>Configurazione iniziale del server SSH

Per configurare il server OpenSSH per l'uso iniziale in Windows, avviare PowerShell come amministratore, quindi eseguire i comandi seguenti per avviare il servizio SSHD:

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup. 
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled 
```

## <a name="initial-use-of-ssh"></a>Uso iniziale di SSH

Dopo aver installato il server OpenSSH in Windows, è possibile testarlo rapidamente usando PowerShell da qualsiasi dispositivo Windows con il client SSH installato. In PowerShell digitare il comando seguente: 

```powershell
Ssh username@servername
```

La prima connessione a qualsiasi server comporterà un messaggio simile al seguente:

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

La risposta deve essere "Yes" o "No". Se si risponde Sì, il server viene aggiunto all'elenco di host SSH noti del sistema locale.

A questo punto verrà richiesta la password. Per motivi di sicurezza, la password non verrà visualizzata durante la digitazione. 

Una volta eseguita la connessione, verrà visualizzato un prompt della shell dei comandi simile al seguente:

```
domain\username@SERVERNAME C:\Users\username>
```

La shell predefinita utilizzata da Windows OpenSSH server è la shell dei comandi di Windows. 

