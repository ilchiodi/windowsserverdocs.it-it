---
ms.date: 09/27/2019
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, installare, configurare
contributor: maertendMSFT
author: maertendMSFT
title: Installazione di OpenSSH per Windows
ms.openlocfilehash: 9cf87229f5ebde6f0ff52a4e9b1b11b6e3ed4f0a
ms.sourcegitcommit: 8771a9f5b37b685e49e2dd03c107a975bf174683
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2020
ms.locfileid: "76145917"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>Installazione di OpenSSH per Windows Server 2019 e Windows 10 #

Il client OpenSSH e il server OpenSSH sono componenti installabili separatamente in Windows Server 2019 e Windows 10 1809.
Gli utenti con queste versioni di Windows devono usare le istruzioni seguenti per installare e configurare OpenSSH. 

> [!NOTE] 
> Gli utenti che hanno acquisito OpenSSH dal repository GitHub di PowerShell (https://github.com/PowerShell/OpenSSH-Portable) devono usare le istruzioni disponibili nel repository e __non__ queste istruzioni. 


## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>Installazione di OpenSSH dall'interfaccia utente Impostazioni in Windows Server 2019 o Windows 10 1809

Il client e il server OpenSSH sono funzionalità installabili di Windows 10 1809. 

Per installare OpenSSH, avvia Impostazioni e quindi passa ad App > App e funzionalità > Gestisci funzionalità facoltative. 

Analizza questo elenco per verificare se il client OpenSSH è già installato. Se non è installato, nella parte superiore della pagina seleziona "Aggiungi una funzionalità" e quindi: 

* Per installare il client OpenSSH, individua "Client OpenSSH" e quindi fai clic su "Installa". 
* Per installare il server OpenSSH, individua "Server OpenSSH" e quindi fai clic su "Installa". 

Al termine dell'installazione torna ad App > App e funzionalità > Gestisci funzionalità facoltative e noterai che i componenti di OpenSSH sono presenti nell'elenco.

> [!NOTE]
> L'installazione del server OpenSSH crea e abilita una regola del firewall denominata "OpenSSH-Server-In-TCP". Questa regola consente il traffico SSH in ingresso sulla porta 22. 

## <a name="installing-openssh-with-powershell"></a>Installazione di OpenSSH con PowerShell 

Per installare OpenSSH con PowerShell, avvia innanzitutto PowerShell come amministratore.
Per verificare che le funzionalità di OpenSSH siano disponibili per l'installazione:

```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'

# This should return the following output:

Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent
Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

Quindi installa le funzionalità del server e/o del client:

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

Per disinstallare OpenSSH usando le impostazioni di Windows, avvia Impostazioni e quindi passa ad App > App e funzionalità > Gestisci funzionalità facoltative. Nell'elenco delle funzionalità installate seleziona il componente client OpenSSH o server OpenSSH e quindi Disinstalla.

Per disinstallare OpenSSH usando PowerShell, usa uno dei comandi seguenti:

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

Dopo la rimozione di OpenSSH, può essere necessario riavviare Windows se il servizio è in uso al momento della disinstallazione.


## <a name="initial-configuration-of-ssh-server"></a>Configurazione iniziale del server SSH

Per configurare il server OpenSSH per l'uso iniziale in Windows, avvia PowerShell come amministratore e quindi esegui i comandi seguenti per avviare il servizio SSHD:

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup. 
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled
# If the firewall does not exist, create one
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```

## <a name="initial-use-of-ssh"></a>Uso iniziale di SSH

Dopo aver installato il server OpenSSH in Windows, puoi testarlo rapidamente usando PowerShell da qualsiasi dispositivo Windows con il client SSH installato. In PowerShell digita il comando seguente: 

```powershell
Ssh username@servername
```

La prima connessione a qualsiasi server restituirà un messaggio simile al seguente:

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

La risposta deve essere "Sì" o "No". Se rispondi Sì, il server viene aggiunto all'elenco di host SSH noti del sistema locale.

A questo punto viene richiesta la password. Per motivi di sicurezza, la password non verrà visualizzata durante la digitazione. 

Una volta eseguita la connessione, visualizzerai un prompt della shell dei comandi simile al seguente:

```
domain\username@SERVERNAME C:\Users\username>
```

La shell predefinita usata dal server OpenSSH per Windows è la shell dei comandi di Windows. 

