---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, install, Setup
contributor: maertendMSFT
author: maertendMSFT
title: Configurazione del server OpenSSH per Windows
ms.product: w10
ms.openlocfilehash: ed9f3653c79f1329b1334f52fe14c1184bc99539
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866864"
---
# <a name="openssh-key-management"></a>Gestione delle chiavi OpenSSH

La maggior parte dell'autenticazione negli ambienti Windows viene eseguita con una coppia nome utente-password.
Questo è adatto per i sistemi che condividono un dominio comune. Quando si lavora tra domini diversi, ad esempio tra sistemi locali e ospitati nel cloud, diventa più difficile.

Per confronto, gli ambienti Linux usano comunemente coppie di chiavi pubbliche/private chiave per l'autenticazione.
OpenSSH include gli strumenti per supportare questa operazione, in particolare:

* __ssh-keygen__ per la generazione di chiavi sicure
* __ssh-agent__ e __ssh-aggiungere__ per archiviare in modo sicuro le chiavi private
* __SCP__ e __SFTP__ per copiare in modo sicuro i file di chiave pubblica durante l'utilizzo iniziale di un server

Questo documento fornisce una panoramica su come usare questi strumenti in Windows per iniziare a usare l'autenticazione della chiave con SSH. Se non si ha familiarità con la gestione delle chiavi SSH, è consigliabile esaminare il [documento NIST IR 7966](http://nvlpubs.nist.gov/nistpubs/ir/2015/NIST.IR.7966.pdf) intitolato "Security of Interactive and Automatic Access management using Secure Shell (SSH)".

## <a name="about-key-pairs"></a>Informazioni sulle coppie di chiavi

Le coppie di chiavi fanno riferimento ai file di chiave pubblica e privata usati da determinati protocolli di autenticazione. 

L'autenticazione con chiave pubblica SSH usa algoritmi di crittografia asimmetrici per generare due file di chiave, uno "privato" e l'altro "pubblico". I file della chiave privata sono l'equivalente di una password e devono essere protetti in tutte le circostanze. Se un utente acquisisce la chiave privata, può accedere a qualsiasi server SSH a cui si ha accesso. La chiave pubblica viene inserita nel server SSH e può essere condivisa senza compromettere la chiave privata.

Quando si usa l'autenticazione con chiave con un server SSH, il server SSH e il client confrontano la chiave pubblica per il nome utente fornito con la chiave privata. Se non è possibile convalidare la chiave pubblica rispetto alla chiave privata sul lato client, l'autenticazione ha esito negativo. 

È possibile implementare l'autenticazione a più fattori con coppie di chiavi richiedendo che venga fornita una passphrase quando viene generata la coppia di chiavi (vedere generazione di chiavi di seguito). Durante l'autenticazione, all'utente viene richiesto di specificare la passphrase, che viene usata insieme alla presenza della chiave privata nel client SSH per autenticare l'utente. 

## <a name="host-key-generation"></a>Generazione chiave host

Le chiavi pubbliche hanno requisiti ACL specifici che, in Windows, equivalgono a consentire solo l'accesso agli amministratori e al sistema. Per semplificare questa operazione, 

* Il modulo PowerShell OpenSSHUtils è stato creato per impostare correttamente gli ACL della chiave e deve essere installato nel server
* Al primo utilizzo di sshd, la coppia di chiavi per l'host verrà generata automaticamente. Se ssh-agent è in esecuzione, le chiavi verranno aggiunte automaticamente all'archivio locale. 

Per semplificare l'autenticazione della chiave con un server SSH, eseguire i comandi seguenti da un prompt di PowerShell con privilegi elevati:

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

Per usare l'autenticazione basata su chiavi, è prima di tutto necessario generare alcune coppie di chiavi pubbliche/private per il client. Da PowerShell o cmd usare ssh-keygen per generare alcuni file di chiave.

```powershell
cd ~\.ssh\
ssh-keygen
```

Verrà visualizzata una schermata simile alla seguente (dove "username" viene sostituito dal nome utente)

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\username\.ssh\id_ed25519):
```

È possibile premere INVIO per accettare il valore predefinito o specificare un percorso in cui si desidera che vengano generate le chiavi. A questo punto, verrà richiesto di usare una passphrase per crittografare i file di chiave privata.
La passphrase funziona con il file di chiave per fornire l'autenticazione a due fattori. Per questo esempio, la passphrase viene lasciata vuota. 

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

A questo punto si dispone di una coppia di chiavi ED25519 pubblica/privata (i file. pub sono chiavi pubbliche e le altre sono chiavi private):

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/28/2018  11:09 AM           1679 id_ed25519
-a----        9/28/2018  11:09 AM            414 id_ed25519.pub
```

Tenere presente che i file di chiave privata sono l'equivalente di una password deve essere protetta nello stesso modo in cui si protegge la password.
A questo proposito, usare ssh-agent per archiviare in modo sicuro le chiavi private in un contesto di sicurezza di Windows, associato all'account di accesso di Windows. A tale scopo, avviare il servizio ssh-agent come amministratore e usare ssh-add per archiviare la chiave privata. 

```powershell
# Make sure you're running as an Administrator
Start-Service ssh-agent

# This should return a status of Running
Get-Service ssh-agent

# Now load your key files into ssh-agent
ssh-add ~\.ssh\id_ed25519

```

Dopo aver completato questi passaggi, ogni volta che è necessaria una chiave privata per l'autenticazione da questo client, ssh-agent recupererà automaticamente la chiave privata locale e la passerà al client SSH.

> [!NOTE]
> Si consiglia vivamente di eseguire il backup della chiave privata in un percorso sicuro, quindi di eliminarla dal sistema locale, *dopo averla* aggiunta a ssh-agent.
> Impossibile recuperare la chiave privata dall'agente.
> Se si perde l'accesso alla chiave privata, è necessario creare una nuova coppia di chiavi e aggiornare la chiave pubblica in tutti i sistemi con cui si interagisce.

## <a name="deploying-the-public-key"></a>Distribuzione della chiave pubblica

Per usare la chiave utente creata in precedenza, la chiave pubblica deve essere inserita nel server in un file di testo denominato *authorized_keys* in users\username\ssh. Gli strumenti OpenSSH includono SCP, un'utilità di trasferimento di file sicura, per semplificare questa operazione.

Per spostare il contenuto della chiave pubblica (~\.ssh\id_ed25519.pub) in un file di testo denominato authorized_keys in ~\.SSH \ sul server/host.

Questo esempio usa la funzione Repair-AuthorizedKeyPermissions nel modulo OpenSSHUtils precedentemente installato nell'host nelle istruzioni riportate sopra.

```powershell
# Make sure that the .ssh directory exists in your server's home folder
ssh user1@domain1@contoso.com mkdir C:\users\user1\.ssh\

# Use scp to opy the public key file generated previously to authorized_keys on your server
scp C:\Users\user1\.ssh\id_ed25519.pub user1@domain1@contoso.com:C:\Users\user1\.ssh\authorized_keys

# Appropriately ACL the authorized_keys file on your server  
ssh --% user1@domain1@contoso.com powershell -c $ConfirmPreference = 'None'; Repair-AuthorizedKeyPermission C:\Users\user1\.ssh\authorized_keys
```

Questa procedura completa la configurazione necessaria per usare l'autenticazione basata su chiavi con SSH in Windows.
Successivamente, l'utente può connettersi all'host SSHD da qualsiasi client che disponga della chiave privata.

