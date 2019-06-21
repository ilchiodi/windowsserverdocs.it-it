---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, installare, configurare
contributor: maertendMSFT
ms.product: w10
author: maertendMSFT
title: Configurazione di OpenSSH Server per Windows
ms.openlocfilehash: 7eff3d3e1af67c9daf7a68c67c3609c0ee89fc93
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280029"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>Configurazione di OpenSSH Server per Windows 10 1809 e Server & 2019

Questo argomento illustra la configurazione di Windows specifici per OpenSSH Server (sshd). 

OpenSSH mantiene la documentazione dettagliata per opzioni di configurazione in linea [OpenSSH.com](https://www.openssh.com/manual.html), non ovvero essere duplicato in questo set di documentazione. 

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>Configurare la shell predefinita per OpenSSH in Windows

La shell dei comandi predefinita offre l'esperienza che utente vede quando ci si connette al server tramite SSH. Il valore predefinito iniziale Windows è la shell dei comandi di Windows (cmd.exe). Windows include anche Bash e PowerShell e shell dei comandi di terze parti sono disponibili anche per Windows e può essere configurato come la shell predefinita per un server.

Per impostare il valore predefinito di shell dei comandi, verificare che la cartella di installazione di OpenSSH sia nel percorso di sistema. Per Windows, la cartella di installazione predefinita è SystemDrive:WindowsDirectory\System32\openssh. I comandi seguenti viene illustrata l'impostazione del percorso corrente e aggiungervi la cartella di installazione predefinita OpenSSH. 

Shell dei comandi | Comando da utilizzare
------------- | -------------- 
Comando | path
PowerShell | $env:path

La configurazione predefinita ssh shell viene effettuata nel Registro di sistema Windows, aggiungendo il percorso completo alla shell Computer\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH eseguibile nel valore della stringa DefaultShell. 

Ad esempio, il comando Powershell seguente imposta la shell predefinita per essere PowerShell.exe:

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshdconfig"></a>Configurazioni di Windows in sshd_config 

Sshd legge i dati di configurazione da %programdata%\ssh\sshd_config per impostazione predefinita in Windows, oppure può essere specificato un file di configurazione diverse da avvio sshd.exe con il parametro -f.
Se il file è assente, sshd generata automaticamente una con la configurazione predefinita quando viene avviato il servizio.

Gli elementi elencati di seguito forniscono possibile configurazione di Windows specifiche attraverso le voci in sshd_config. Esistono altre impostazioni di configurazione possibili in cui non sono elencati in questo caso, come vengono trattati in dettaglio nella online [OpenSSH-Win32 documentazione](https://github.com/powershell/win32-openssh/wiki). 


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups, AllowUsers, DenyGroups, DenyUsers 

Controllare quali utenti e gruppi possono connettersi al server viene eseguita usando le direttive AllowGroups, AllowUsers, DenyGroups e DenyUsers. Le direttive Consenti/Nega vengono elaborate nell'ordine seguente: DenyUsers AllowUsers, DenyGroups e infine AllowGroups. Tutti i nomi di account devono essere specificati in caratteri minuscoli. Visualizzare i modelli in ssh_config per altre informazioni sui modelli per i caratteri jolly.

Durante la configurazione di utenti o gruppi basata su regole con un utente di dominio o un gruppo, usare il formato seguente: ``` user?domain* ```.
Windows consente a più formati che consentono di specificare le entità di dominio, ma molti sono in conflitto con i modelli di Linux standard. Per questo motivo, * viene aggiunto per coprire gli FQDN. Inoltre, questo approccio Usa "?", invece di @, per evitare conflitti con i username@host formato. 

Gruppo di lavoro utenti/gruppi e account connesso a internet vengono risolti sempre al relativo nome di account locale (nessuna parte del dominio, simile a nomi standard Unix). Gli utenti del dominio e i gruppi sono rigorosamente risolti [NameSamCompatible](https://docs.microsoft.com/windows/desktop/api/secext/ne-secext-extended_name_format) formato - domain_short_name\user_name. Utente o gruppo tutti basati su regole devono essere conformi al formato di questa configurazione.

Esempi per gli utenti del dominio e gruppi 

```
DenyUsers contoso\admin@192.168.2.23 : blocks contoso\admin from 192.168.2.23
DenyUsers contoso\* : blocks all users from contoso domain
AllowGroups contoso\sshusers : only allow users from contoso\sshusers group
```

Esempi per utenti e gruppi locali 

```
AllowUsers localuser@192.168.2.23
AllowGroups sshusers
```

### <a name="authenticationmethods"></a>AuthenticationMethods 

Per Windows OpenSSH, i metodi di autenticazione è disponibile solo sono "password" e "chiave pubblica".

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile 

Il valore predefinito è ". SSH/authorized_keys. SSH/authorized_keys2". Se il percorso non è assoluto, verrà considerato relativo alla home directory dell'utente (o percorso immagine del profilo). Ad esempio: c:\users\user.

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory (supporto aggiunto nella v7.7.0.0)

Questa direttiva è supportata solo con le sessioni di sftp. Una sessione remota cmd.exe non rispetta questo. Per configurare un server sftp sola chroot, impostare ForceCommand interno sftp. Può anche configurare scp con chroot, implementando una shell personalizzata che avrebbe consentito solamente scp e sftp.

### <a name="hostkey"></a>HostKey

I valori predefiniti sono programdata%/ssh/ssh_host_rsa_key programdata%/ssh/ssh_host_ecdsa_key, %programdata%/ssh/ssh_host_ed25519_key e % %. Se le impostazioni predefinite non sono presenti, sshd genera automaticamente queste un avvio del servizio.

### <a name="match"></a>Corrispondenza

Si noti che Criteri di regole in questa sezione. I nomi utente e gruppo devono essere in lettere minuscole.

### <a name="permitrootlogin"></a>PermitRootLogin

Non applicabile in Windows. Per impedire l'accesso dell'amministratore, usare gli amministratori con DenyGroups direttiva.

### <a name="syslogfacility"></a>SyslogFacility

Se è necessaria la registrazione basata su file, usare LOCAL0. I log vengono generati in % programdata%\ssh\logs.
Qualsiasi altro valore, incluso il valore predefinito AUTH indirizza la registrazione di ETW. Per altre informazioni, vedere le funzioni di registrazione in Windows.

### <a name="not-supported"></a>Non supportato 

Le opzioni di configurazione seguenti non sono disponibili nella versione OpenSSH fornito con Windows Server 2019 e Windows 10 1809:

* AcceptEnv
* AllowStreamLocalForwarding
* AuthorizedKeysCommand
* AuthorizedKeysCommandUser
* AuthorizedPrincipalsCommand
* AuthorizedPrincipalsCommandUser
* Compressione
* ExposeAuthInfo
* GSSAPIAuthentication
* GSSAPICleanupCredentials
* GSSAPIStrictAcceptorCheck
* HostbasedAcceptedKeyTypes
* HostbasedAuthentication
* HostbasedUsesNameFromPacketOnly
* IgnoreRhosts
* IgnoreUserKnownHosts
* KbdInteractiveAuthentication
* KerberosAuthentication
* KerberosGetAFSToken
* KerberosOrLocalPasswd
* KerberosTicketCleanup
* PermitTunnel
* PermitUserEnvironment
* PermitUserRC
* PidFile
* PrintLastLog
* RDomain
* StreamLocalBindMask
* StreamLocalBindUnlink
* StrictModes
* X11DisplayOffset
* X11Forwarding
* X11UseLocalhost
* XAuthLocation

