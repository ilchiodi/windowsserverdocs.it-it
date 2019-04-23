---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, installare, configurare
contributor: maertendMSFT
author: maertendMSFT
title: Configurazione di OpenSSH Server per Windows
ms.product: w10
ms.openlocfilehash: 3e2981cbbdfe34bf28a77d5121bff0b663e0d3bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837152"
---
# <a name="openssh-key-management"></a>Gestione delle chiavi OpenSSH

La maggior parte dell'autenticazione in ambienti Windows viene eseguita con una coppia di nome utente-password.
Ciò vale per i sistemi che condividono un dominio comune. Quando si lavora in domini, ad esempio tra sistemi locali e ospitate nel cloud, diventa più difficile.

In confronto, gli ambienti Linux usare comunemente coppie di chiave pubblica/chiave privata per l'autenticazione di unità.
OpenSSH include gli strumenti per supportare questa operazione, in particolare:

* __SSH-keygen__ per generare le chiavi di sicurezza
* __SSH-agent__ e __ssh-add__ per archiviare in modo sicuro le chiavi private
* __SCP__ e __sftp__ da copiare in modo sicuro i file di chiave pubblici durante l'utilizzo iniziale di un server

Questo documento fornisce una panoramica su come usare questi strumenti in Windows per iniziare a usare l'autenticazione con chiave con SSH. Se non si ha familiarità con la gestione delle chiavi SSH, è consigliabile esaminare [documento di NIST 7966 IR](http://nvlpubs.nist.gov/nistpubs/ir/2015/NIST.IR.7966.pdf) intitolato "Sicurezza di Interactive e automatizzata Access Management Using Secure Shell (SSH)".

## <a name="about-key-pairs"></a>Sulle coppie di chiavi

Coppie di chiavi, consultare i file di chiave pubblici e privati utilizzati da alcuni protocolli di autenticazione. 

L'autenticazione con chiave pubblica SSH Usa algoritmi di crittografia asimmetrici per generare due file di chiavi: una "privata" e altri "pubblico". I file di chiave privata sono l'equivalente di una password e devono protette in qualsiasi circostanza. Se un utente acquisisce la chiave privata, possono registrare come a qualsiasi server SSH che è possibile utilizzare. La chiave pubblica è ciò che viene inserito nel server SSH e può essere condiviso senza compromettere la chiave privata.

Quando si usa l'autenticazione con chiave con un server SSH, il server SSH e il client confronta la chiave pubblica per il nome utente fornito con la chiave privata. Se la chiave pubblica non può essere convalidata rispetto la chiave privata del client, l'autenticazione ha esito negativo. 

Multi-factor authentication possono essere implementate con coppie di chiavi da che richiede una passphrase quando viene generata la coppia di chiavi (vedere la generazione della chiave seguente). Durante l'autenticazione che all'utente viene chiesta la passphrase, che viene usato insieme la presenza della chiave privata del client SSH per autenticare l'utente. 

## <a name="host-key-generation"></a>Generazione della chiave host

Le chiavi pubbliche hanno ACL requisiti specifici che, in Windows, equivalgono a consentire solo l'accesso al sistema e gli amministratori. Per semplificare questa operazione, 

* Il modulo OpenSSHUtils PowerShell è stato creato per impostare la chiave di ACL in modo corretto e deve essere installato nel server
* Al primo utilizzo del disco sshd, la coppia di chiavi per l'host verrà generata automaticamente. Se ssh-agent è in esecuzione, le chiavi verranno automaticamente aggiunto all'archivio locale. 

Per facilitare l'autenticazione con chiave con un server SSH, eseguire i comandi seguenti da un prompt di PowerShell con privilegi elevato:

```powershell

# Install the OpenSSHUtils module to the server. This will be valuable when deploying user keys.
Install-Module -Force OpenSSHUtils -Scope AllUsers

# Start the ssh-agent service to preserve the server keys
Start-Service ssh-agent

# Now start the sshd service
Start-Service sshd
```

Poiché non esiste alcun utente associato al servizio sshd, le chiavi host vengono archiviate in \ProgramData\ssh.


## <a name="user-key-generation"></a>Generazione della chiave utente

Per usare l'autenticazione basata su chiavi, è necessario innanzitutto generare alcune coppie di chiavi pubblica/privata per il client. Da PowerShell o cmd, usare strumenti quali ssh-keygen per generare alcuni file di chiave.

```powershell
cd ~\.ssh\
ssh-keygen
```

Questo dovrebbe essere simile al seguente (dove "username" verrà sostituito dal nome dell'utente)

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\username\.ssh\id_ed25519):
```

È possibile premere INVIO per accettare il valore predefinito o specificare un percorso in cui si desidera la generazione di chiavi. A questo punto, verrà richiesto di utilizzare una passphrase per crittografare i file di chiave privata.
La passphrase funziona con il file di chiave per fornire l'autenticazione a 2 fattori. In questo esempio si sta lasciando vuota la passphrase. 

```
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in C:\Users\username\.ssh\id_ed25519.
Your public key has been saved in C:\Users\username\.ssh\id_ed25519.pub.
The key fingerprint is: 
SHA256:OIzc1yE7joL2Bzy8!gS0j8eGK7bYaH1FmF3sDuMeSj8 username@server@LOCAL-HOSTNAME

The key's randomart image is:
+--[ED25519 256]--+
|        .        |
|         o       |
|    . + + .      |
|   o B * = .     |
|   o= B S .      |
|   .=B O o       |
|  + =+% o        |
| *oo.O.E         |
|+.o+=o. .        |
+----[SHA256]-----+
```

È ora disponibile una pubblica/privata ED25519 coppia di chiavi (i file. pub sono le chiavi pubbliche e gli altri sono le chiavi private):

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/28/2018  11:09 AM           1679 id_ed25519
-a----        9/28/2018  11:09 AM            414 id_ed25519.pub
```

Tenere presente che i file di chiave privati sono che l'equivalente di una password deve essere protetto esattamente come si protegge la password.
A che scopo, usare ssh-agent per archiviare in modo sicuro le chiavi private all'interno di un contesto di sicurezza di Windows, associati con l'account di accesso di Windows. A tale scopo, avviare il servizio ssh-agent come amministratore e usare ssh-add per archiviare la chiave privata. 

```powershell
# Make sure you're running as an Administrator
Start-Service ssh-agent

# This should return a status of Running
Get-Service ssh-agent

# Now load your key files into ssh-agent
ssh-add ~\.ssh\id_ed25519

```

Dopo aver completato questi passaggi, ogni volta che per l'autenticazione dal client, è necessaria una chiave privata ssh-agent verrà automaticamente recuperare la chiave privata locale e passarlo al client SSH.

> [!NOTE]
> Si consiglia di eseguire il backup della chiave privata in una posizione sicura, quindi eliminarlo dal sistema locale, *dopo* aggiungendolo a ssh-agent.
> La chiave privata non può essere recuperata dall'agente.
> Se si perde l'accesso alla chiave privata, è necessario creare una nuova coppia di chiavi e aggiornare la chiave pubblica in tutti i sistemi che si interagisce con.

## <a name="deploying-the-public-key"></a>Distribuire la chiave pubblica

Per usare la chiave utente creato in precedenza, la chiave pubblica deve essere inserita nel server in un file di testo denominato *authorized_keys* sotto users\username\ssh. Gli strumenti di OpenSSH includono scp, ovvero un'utilità di trasferimento file sicuro, per facilitare questa operazione.

Per spostare il contenuto della chiave pubblica (~\.ssh\id_ed25519.pub) in un testo file denominato authorized_keys in ~\.ssh\ nell'host/server.

In questo esempio viene utilizzata la funzione di riparazione AuthorizedKeyPermissions nel modulo OpenSSHUtils che in precedenza è stato installato nell'host le istruzioni riportate sopra.

```powershell
# Make sure that the .ssh directory exists in your server's home folder
ssh user1@domain1@contoso.com mkdir C:\users\user1\.ssh\

# Use scp to opy the public key file generated previously to authorized_keys on your server
scp C:\Users\user1\.ssh\id_ed25519.pub user1@domain1@contoso.com:C:\Users\user1\.ssh\authorized_keys

# Appropriately ACL the authorized_keys file on your server  
ssh --% user1@domain1@contoso.com powershell -c $ConfirmPreference = 'None'; Repair-AuthorizedKeyPermission C:\Users\user1\.ssh\authorized_keys
```

Questi passaggi da completare la configurazione necessaria per usare l'autenticazione basata su chiavi con SSH in Windows.
Al termine, l'utente può connettersi all'host del disco sshd da qualsiasi client che dispone della chiave privata.

