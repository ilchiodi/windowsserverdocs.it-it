---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, installazione, configurazione
contributor: maertendMSFT
author: maertendMSFT
title: Configurazione del server OpenSSH per Windows
ms.product: w10
ms.openlocfilehash: fa3d40617a04c092403d9d2e018bd2eb82d20cd9
ms.sourcegitcommit: effbc183bf4b370905d95c975626c1ccde057401
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/03/2019
ms.locfileid: "74781318"
---
# <a name="openssh-key-management"></a>Gestione delle chiavi OpenSSH

Negli ambienti Windows quasi tutte le operazioni di autenticazione vengono eseguite con una coppia nome utente/password.
Questo meccanismo funziona perfettamente per i sistemi che condividono un dominio comune. Quando invece si usano più domini, ad esempio sistemi locali e sistemi ospitati nel cloud, lo scenario diventa più complesso.

Per fare un confronto, gli ambienti Linux usano comunemente coppie di chiavi pubblica/privata per l'autenticazione.
OpenSSH include strumenti per supportare queste operazioni, in particolare:

* __ssh-keygen__ per generare chiavi sicure
* __ssh-agent__ e __ssh-add__ per archiviare le chiavi private in modo sicuro
* __scp__ e __sftp__ per copiare i file di chiave pubblica in modo sicuro durante l'uso iniziale di un server

Questo documento fornisce informazioni generali su come usare questi strumenti in Windows per iniziare a usare l'autenticazione tramite chiave con SSH. Se non hai familiarità con la gestione delle chiavi SSH, ti consigliamo di consultare il [documento NIST IR 7966](http://nvlpubs.nist.gov/nistpubs/ir/2015/NIST.IR.7966.pdf) dal titolo "Security of Interactive and Automated Access Management Using Secure Shell (SSH)" (Sicurezza relativa alla gestione degli accessi interattivi e automatici tramite Secure Shell (SSH)).

## <a name="about-key-pairs"></a>Informazioni sulle coppie di chiavi

Per coppie di chiavi si intendono file di chiave pubblica e privata usati da determinati protocolli di autenticazione. 

L'autenticazione con chiave pubblica SSH usa algoritmi di crittografia asimmetrici per generare due file, uno di chiave privata e l'altro di chiave pubblica. I file di chiave privata equivalgono a una password e devono essere protetti in tutte le situazioni. Se un utente acquisisce la tua chiave privata, può accedere a qualsiasi server SSH accessibile a te. La chiave pubblica è l'elemento che viene inserito nel server SSH e può essere condivisa senza compromettere la chiave privata.

Quando si usa l'autenticazione tramite chiave con un server SSH, il client e il server SSH confrontano la chiave pubblica relativa al nome utente specificato con la chiave privata. Se non è possibile convalidare la chiave pubblica a fronte della chiave privata sul lato client, l'autenticazione non riesce. 

È possibile implementare l'autenticazione a più fattori con coppie di chiavi richiedendo che venga fornita una passphrase quando viene generata la coppia di chiavi (vedi la generazione di chiavi più avanti). Durante l'autenticazione, all'utente viene richiesto di specificare la passphrase, che viene usata insieme alla presenza della chiave privata nel client SSH per autenticare l'utente. 

## <a name="host-key-generation"></a>Generazione di chiavi host

Le chiavi pubbliche hanno requisiti ACL specifici che, in Windows, equivalgono a consentire l'accesso solo agli amministratori e al sistema. Per semplificare: 

* Il modulo PowerShell OpenSSHUtils è stato creato per impostare correttamente gli ACL delle chiavi e deve essere installato nel server
* Al primo uso di sshd, la coppia di chiavi per l'host verrà generata automaticamente. Se ssh-agent è in esecuzione, le chiavi verranno aggiunte automaticamente all'archivio locale. 

Per semplificare l'autenticazione tramite chiave con un server SSH, esegui i comandi seguenti da un prompt di PowerShell con privilegi elevati:

```powershell

# Install the OpenSSHUtils module to the server. This will be valuable when deploying user keys.
Install-Module -Force OpenSSHUtils -Scope AllUsers

# Start the ssh-agent service to preserve the server keys
Start-Service ssh-agent

# Now start the sshd service
Start-Service sshd
```

Poiché al servizio sshd non è associato alcun utente, le chiavi host vengono archiviate in \ProgramData\ssh.


## <a name="user-key-generation"></a>Generazione di chiavi utente

Per usare l'autenticazione basata su chiavi, devi prima generare alcune coppie di chiavi pubblica/privata per il client. In PowerShell o cmd usa ssh-keygen per generare alcuni file di chiave.

```powershell
cd ~\.ssh\
ssh-keygen
```

Verrà visualizzata una schermata simile alla seguente (dove "username" è sostituito dal tuo nome utente).

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\username\.ssh\id_ed25519):
```

Puoi premere INVIO per accettare il valore predefinito oppure specificare un percorso in cui vuoi che vengano generate le chiavi. A questo punto, ti verrà richiesto di usare una passphrase per crittografare i file di chiave privata.
La passphrase in combinazione con il file di chiave fornisce l'autenticazione a due fattori. Per questo esempio, la passphrase viene lasciata vuota. 

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

A questo punto, hai una coppia ED25519 di chiavi pubblica/privata (i file con estensione pub sono chiavi pubbliche, mentre le altre sono chiavi private):

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/28/2018  11:09 AM           1679 id_ed25519
-a----        9/28/2018  11:09 AM            414 id_ed25519.pub
```

Tieni presente che i file di chiave privata equivalgono a una password e devono essere protetti allo stesso modo.
A questo proposito, usa ssh-agent per archiviare in modo sicuro le chiavi private in un contesto di sicurezza di Windows, insieme all'account di accesso di Windows. A tale scopo, avvia il servizio ssh-agent come amministratore e usa ssh-add per archiviare la chiave privata. 

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
> È consigliabile eseguire il backup della chiave privata in una posizione sicura, eliminandola successivamente dal sistema locale *dopo* averla aggiunta a ssh-agent.
> La chiave privata non può essere recuperata dall'agente.
> Se non sei più in grado di accedere alla chiave privata, dovrai creare una nuova coppia di chiavi e aggiornare la chiave pubblica in tutti i sistemi con cui interagisci.

## <a name="deploying-the-public-key"></a>Distribuzione della chiave pubblica

Per usare la chiave utente creata in precedenza, è necessario inserire la chiave pubblica nel server in un file di testo denominato *authorized_keys* in users\username\.ssh\.. Per facilitare questa operazione, gli strumenti OpenSSH includono scp, un'utilità di trasferimento file sicura.

Sposta il contenuto della chiave pubblica (~\.ssh\id_ed25519.pub) in un file di testo denominato authorized_keys in ~\.ssh\ su server/host.

Questo esempio usa la funzione Repair-AuthorizedKeyPermissions disponibile nel modulo OpenSSHUtils precedentemente installato nell'host in base alle istruzioni riportate sopra.

```powershell
# Make sure that the .ssh directory exists in your server's home folder
ssh user1@domain1@contoso.com mkdir C:\users\user1\.ssh\

# Use scp to opy the public key file generated previously to authorized_keys on your server
scp C:\Users\user1\.ssh\id_ed25519.pub user1@domain1@contoso.com:C:\Users\user1\.ssh\authorized_keys

# Appropriately ACL the authorized_keys file on your server  
ssh --% user1@domain1@contoso.com powershell -c $ConfirmPreference = 'None'; Repair-AuthorizedKeyPermission C:\Users\user1\.ssh\authorized_keys
```

Con questi passaggi viene completata la configurazione necessaria per usare l'autenticazione basata su chiavi con SSH in Windows.
Successivamente, l'utente può connettersi all'host sshd da qualsiasi client contenente la chiave privata.

