---
ms.date: 09/27/2018
ms.topic: conceptual
contributor: maertendMSFT
ms.product: windows-server
author: maertendmsft
title: Configurazione del server OpenSSH per Windows
ms.openlocfilehash: 61f176f7f73495a6b9dbbcb1a25f2337a44ab99b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852024"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>Configurazione del server OpenSSH per Windows 10 1809 e Server 2019

Questo argomento descrive la configurazione specifica di Windows per il server OpenSSH (SSHD). 

La documentazione dettagliata per le opzioni di configurazione di OpenSSH è disponibile online all'indirizzo [OpenSSH.com](https://www.openssh.com/manual.html) e non è duplicata in questo set di documentazione. 

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>Configurazione della shell predefinita per OpenSSH in Windows

La shell dei comandi predefinita fornisce l'esperienza visualizzata da un utente quando esegue la connessione al server tramite SSH. La shell predefinita iniziale in Windows è la shell dei comandi di Windows (cmd.exe). Windows include anche shell dei comandi di PowerShell, Bash e terze parti che possono essere configurate come shell predefinite per un server.

Per impostare la shell dei comandi predefinita, verifica innanzitutto che la cartella di installazione di OpenSSH si trovi nel percorso di sistema. Per Windows, la cartella di installazione predefinita è SystemDrive:WindowsDirectory\System32\openssh. I comandi seguenti illustrano l'impostazione del percorso corrente e aggiungono la cartella di installazione di OpenSSH predefinita al percorso. 

Shell dei comandi | Comando da usare
------------- | -------------- 
Comando | path
PowerShell | $env:path

La configurazione della shell SSH predefinita viene eseguita nel registro di sistema di Windows aggiungendo il percorso completo all'eseguibile della shell in Computer\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH nel valore stringa DefaultShell. 

Ad esempio, il comando di PowerShell seguente imposta la shell predefinita come PowerShell.exe:

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshd_config"></a>Configurazioni di Windows in sshd_config 

In Windows, SSHD legge i dati di configurazione da %ProgramData%\ssh\sshd_config per impostazione predefinita. In alternativa, puoi specificare un file di configurazione diverso avviando sshd.exe con il parametro -f.
Se il file è assente, SSHD ne genera uno con la configurazione predefinita all'avvio del servizio.

Gli elementi elencati di seguito forniscono una configurazione specifica di Windows possibile tramite le voci in sshd_config. Sono disponibili altre impostazioni di configurazione che non sono elencate in questo argomento, in quanto sono descritte in dettaglio nella [documentazione online di OpenSSH Win32](https://github.com/powershell/win32-openssh/wiki). 


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups, AllowUsers, DenyGroups, DenyUsers 

Il controllo degli utenti e dei gruppi che possono connettersi al server viene eseguito tramite le direttive AllowGroups, AllowUsers, DenyGroups e DenyUsers. Le direttive Allow/Deny vengono elaborate nell'ordine seguente: DenyUsers, AllowUsers, DenyGroups e infine AllowGroups. Tutti i nomi degli account devono essere specificati in lettere minuscole. Per altre informazioni sui modelli per i caratteri jolly, vedi PATTERNS in ssh_config.

Quando configuri regole basate su utenti o gruppi con un utente o un gruppo di dominio, usa il formato seguente: ``` user?domain* ```.
Windows consente più formati per specificare le entità di dominio, ma molti sono in conflitto con i modelli standard di Linux. Per questo motivo, * viene aggiunto per coprire i nomi di dominio completi. Questo approccio usa anche "?", anziché @, per evitare conflitti con il formato username@host. 

Gli utenti o gruppi del gruppo di lavoro e gli account connessi a Internet vengono sempre risolti con il nome dell'account locale (nessuna parte del dominio, simile ai nomi UNIX standard). Utenti e gruppi di dominio vengono risolti in modo rigoroso nel formato [NameSamCompatible](https://docs.microsoft.com/windows/desktop/api/secext/ne-secext-extended_name_format): nome_breve_dominio\nome_utente. Tutte le regole di configurazione basate su utenti o gruppi devono rispettare questo formato.

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

Per Windows OpenSSH, gli unici metodi di autenticazione disponibili sono "password" e "chiave pubblica".

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile 

Il valore predefinito è ".ssh/authorized_keys .ssh/authorized_keys2". Se il percorso non è assoluto, viene considerato relativo alla directory radice dell'utente (o al percorso dell'immagine del profilo). Ad esempio, C:\Utenti\Utente. Si noti che, se l'utente appartiene al gruppo di amministratori, viene invece usato %programdata%/ssh/administrators_authorized_keys.

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory (supporto aggiunto nella versione 7.7.0.0)

Questa direttiva è supportata solo con le sessioni SFTP. Una sessione remota in cmd.exe non userà questa direttiva. Per configurare un server chroot solo SFTP, imposta ForceCommand su Internal-SFTP. Puoi anche configurare SCP con chroot, implementando una shell personalizzata che consenta solo SCP e SFTP.

### <a name="hostkey"></a>HostKey

Le impostazioni predefinite sono %programdata%/ssh/ssh_host_ecdsa_key, %programdata%/ssh/ssh_host_ed25519_key, %programdata%/ssh/ssh_host_dsa_key e %programdata%/ssh/ssh_host_rsa_key. Se le impostazioni predefinite non sono presenti, SSHD le genera automaticamente all'avvio di un servizio.

### <a name="match"></a>Corrispondenza

Tieni presenti le regole del modello in questa sezione. I nomi di utenti e gruppi devono essere in lettere minuscole.

### <a name="permitrootlogin"></a>PermitRootLogin

Non applicabili in Windows. Per impedire l'accesso come amministratore, usa Administrators con la direttiva DenyGroups.

### <a name="syslogfacility"></a>SyslogFacility

Se è necessaria la registrazione basata su file, usa LOCAL0. I log vengono generati in %programdata%\ssh\logs.
Qualsiasi altro valore, incluso il valore predefinito AUTH, indirizza la registrazione a ETW. Per altre informazioni, vedi la pagina relativa alle funzionalità di registrazione di Windows.

### <a name="not-supported"></a>Non supportato 

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

