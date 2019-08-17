---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, install, Setup
contributor: maertendMSFT
ms.product: w10
author: maertendMSFT
title: Configurazione del server OpenSSH per Windows
ms.openlocfilehash: ed424c33c4cd2c19a9b5e985ab6083bcbcb9fbdc
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546259"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>Configurazione del server OpenSSH per Windows 10 1809 e server 2019

In questo argomento viene illustrata la configurazione specifica di Windows per il server OpenSSH (SSHD). 

OpenSSH mantiene la documentazione dettagliata per le opzioni di configurazione online in [OpenSSH.com](https://www.openssh.com/manual.html), che non viene duplicata in questo set di documentazione. 

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>Configurazione della shell predefinita per OpenSSH in Windows

La shell dei comandi predefinita fornisce l'esperienza visualizzata da un utente per la connessione al server tramite SSH. La finestra predefinita iniziale è la shell dei comandi di Windows (cmd. exe). Windows include anche PowerShell e bash e shell dei comandi di terze parti sono disponibili per Windows e possono essere configurati come shell predefinita per un server.

Per impostare la shell dei comandi predefinita, verificare innanzitutto che la cartella di installazione di OpenSSH si trovi nel percorso di sistema. Per Windows, la cartella di installazione predefinita è SystemDrive: WindowsDirectory\System32\openssh. I comandi seguenti illustrano l'impostazione del percorso corrente e aggiungono alla cartella di installazione di OpenSSH predefinita. 

Shell comandi | Comando da usare
------------- | -------------- 
Comando | path
PowerShell | $env:p ATH

La configurazione della shell SSH predefinita viene eseguita nel registro di sistema di Windows aggiungendo il percorso completo dell'eseguibile della shell a Computer\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH nel valore stringa DefaultShell. 

Ad esempio, il comando di PowerShell seguente imposta la shell predefinita come PowerShell. exe:

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshd_config"></a>Configurazioni di Windows in sshd_config 

In Windows, sshd legge i dati di configurazione da%ProgramData%\ssh\sshd_config per impostazione predefinita oppure è possibile specificare un file di configurazione diverso avviando sshd. exe con il parametro-f.
Se il file è assente, sshd ne genera uno con la configurazione predefinita all'avvio del servizio.

Gli elementi elencati di seguito forniscono una configurazione specifica di Windows possibile tramite le voci in sshd_config. Sono disponibili altre impostazioni di configurazione che non sono elencate in questo argomento, perché sono descritte in dettaglio nella documentazione di [OpenSSH](https://github.com/powershell/win32-openssh/wiki)in linea di Win32. 


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups, AllowUsers, DenyGroups, DenyUsers 

Il controllo degli utenti e dei gruppi che possono connettersi al server viene eseguito tramite le direttive AllowGroups, AllowUsers, DenyGroups e DenyUsers. Le direttive Allow/Deny vengono elaborate nell'ordine seguente: DenyUsers, AllowUsers, DenyGroups e infine AllowGroups. Tutti i nomi degli account devono essere specificati in lettere minuscole. Per ulteriori informazioni sui modelli per i caratteri jolly, vedere PATTERNs in ssh_config.

Quando si configurano regole basate su utenti e gruppi con un utente o un gruppo di dominio ``` user?domain* ```, usare il formato seguente:.
Windows consente più formati per specificare le entità di dominio, ma molti conflitti con i modelli standard di Linux. Per questo motivo, * viene aggiunto per coprire i nomi di dominio completi. Questo approccio usa anche "?", anziché @, per evitare conflitti con il username@host formato. 

Gli utenti/gruppi del gruppo di lavoro e gli account connessi a Internet vengono sempre risolti con il nome dell'account locale (nessuna parte del dominio, simile ai nomi UNIX standard). Utenti e gruppi di dominio sono stati risolti in modo rigoroso in formato [NameSamCompatible](https://docs.microsoft.com/windows/desktop/api/secext/ne-secext-extended_name_format) -domain_short_name\user_name. Tutte le regole di configurazione basate su utenti e gruppi devono rispettare questo formato.

Esempi per utenti e gruppi di dominio 

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

Per Windows OpenSSH, gli unici metodi di autenticazione disponibili sono "password" e "PublicKey".

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile 

Il valore predefinito è ". ssh/authorized_keys. ssh/authorized_keys2". Se il percorso non è assoluto, viene considerato relativo alla Home directory dell'utente (o al percorso dell'immagine del profilo). Ex. c:\Utenti\nome.

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory (supporto aggiunto in v 7.7.0.0)

Questa direttiva è supportata solo con le sessioni SFTP. Una sessione remota in cmd. exe non verrà rispettata. Per configurare un server chroot solo SFTP, impostare ForceCommand su Internal-SFTP. È anche possibile configurare SCP con chroot, implementando una shell personalizzata che consenta solo SCP e SFTP.

### <a name="hostkey"></a>Chiave host

Le impostazioni predefinite sono% ProgramData%/SSH/ssh_host_ecdsa_key,% ProgramData%/SSH/ssh_host_ed25519_key e% ProgramData%/ssh/ssh_host_rsa_key. Se le impostazioni predefinite non sono presenti, sshd le genera automaticamente all'avvio di un servizio.

### <a name="match"></a>Corrispondenza

Si noti che le regole del modello in questa sezione. I nomi di utenti e gruppi devono essere in lettere minuscole.

### <a name="permitrootlogin"></a>PermitRootLogin

Non applicabile in Windows. Per impedire l'accesso amministratore, usare gli amministratori con la direttiva DenyGroups.

### <a name="syslogfacility"></a>SyslogFacility

Se è necessaria la registrazione basata su file, usare LOCAL0. I log vengono generati in%ProgramData%\ssh\logs.
Qualsiasi altro valore, incluso il valore predefinito AUTH, indirizza la registrazione a ETW. Per ulteriori informazioni, vedere la pagina relativa alle funzionalità di registrazione di Windows.

### <a name="not-supported"></a>Non supportate 

Le opzioni di configurazione seguenti non sono disponibili nella versione di OpenSSH fornita in Windows Server 2019 e Windows 10 1809:

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

