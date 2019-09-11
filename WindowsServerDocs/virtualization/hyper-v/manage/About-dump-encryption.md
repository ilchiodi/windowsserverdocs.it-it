---
title: Informazioni sulla crittografia del dump
description: Viene descritto come crittografare i file di dump e risolvere i problemi di crittografia.
ms.prod: windows-server-threshold
manager: dongill
ms.topic: article
author: larsiwer
ms.asset: b78ab493-e7c3-41f5-ab36-29397f086f32
ms.author: kathydav
ms.date: 11/03/2016
ms.openlocfilehash: d46deee7fc9d911de2a6ee44ae097affe1d658a3
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872137"
---
# <a name="about-dump-encryption"></a>Informazioni sulla crittografia del dump
La crittografia del dump può essere usata per crittografare i dump di arresto anomalo del sistema e i dump Live generati per un sistema. I dump vengono crittografati usando una chiave di crittografia simmetrica generata per ogni dump. Questa chiave viene quindi crittografata usando la chiave pubblica specificata dall'amministratore attendibile dell'host (protezione con chiave di crittografia del dump di arresto anomalo del sistema). In questo modo si garantisce che solo un utente con la chiave privata corrispondente possa decrittografare e quindi accedere al contenuto del dump. Questa funzionalità viene sfruttata in un'infrastruttura sorvegliata.
Nota: Se si configura la crittografia del dump, disabilitare anche Segnalazione errori Windows. WER non può leggere i dump di arresto anomalo crittografati.

# <a name="configuring-dump-encryption"></a>Configurazione della crittografia dump
## <a name="manual-configuration"></a>Configurazione manuale
Per attivare la crittografia del dump utilizzando il registro di sistema, configurare i seguenti valori del registro di sistema in`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl`

| Nome valore | Type | Value |
| ---------- | ---- | ----- |
| DumpEncryptionEnabled | DWORD | 1 per abilitare la crittografia del dump, 0 per disabilitare la crittografia del dump |
| EncryptionCertificates\Certificate.1::P ublicKey | Binary | Chiave pubblica (RSA, 2048 bit) da usare per la crittografia dei dump. Questo deve essere formattato come [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| EncryptionCertificates\Certificate.1:: identificazione personale | String | Identificazione personale del certificato per consentire la ricerca automatica della chiave privata nell'archivio certificati locale durante la decrittografia di un dump di arresto anomalo del sistema. |


## <a name="configuration-using-script"></a>Configurazione tramite script
Per semplificare la configurazione, è disponibile uno [script di esempio](https://github.com/Microsoft/Virtualization-Documentation/tree/live/hyperv-tools/DumpEncryption) per abilitare la crittografia del dump basata su una chiave pubblica di un certificato.

1. In un ambiente attendibile: Creare un certificato con una chiave RSA a 2048 bit ed esportare il certificato pubblico
2. Negli host di destinazione: Importare il certificato pubblico nell'archivio certificati locale
3. Eseguire lo script di configurazione di esempio 
    ```
    .\Set-DumpEncryptionConfiguration.ps1 -Certificate (Cert:\CurrentUser\My\093568AB328DF385544FAFD57EE53D73EFAAF519) -Force
    ```

# <a name="decrypting-encrypted-dumps"></a>Decrittografia di dump crittografati
Per decrittografare un file di dump crittografato esistente, è necessario scaricare e installare gli strumenti di debug per Windows. Questo set di strumenti contiene KernelDumpDecrypt. exe, che può essere usato per decrittografare un file di dump crittografato.
Se il certificato che include la chiave privata è presente nell'archivio certificati dell'utente corrente, è possibile decrittografare il file di dump chiamando

```
    KernelDumpDecrypt.exe memory.dmp memory_decr.dmp
```
Dopo la decrittografia, gli strumenti come WinDbg possono aprire il file dump decrittografato.

# <a name="troubleshooting-dump-encryption"></a>Risoluzione dei problemi di crittografia dump
Se la crittografia del dump è abilitata in un sistema ma non viene generato alcun dump, controllare il registro `System` eventi del sistema `Kernel-IO` per l'evento 1207. Quando non è possibile inizializzare la crittografia del dump, questo evento viene creato e i dump sono disabilitati.

| Messaggio di errore dettagliato | Passaggi per attenuare |
| ---------------------- | ----------------- |
| Registro di sistema di chiave pubblica o identificazione personale mancante | Controllare se i valori del registro di sistema sono presenti nel percorso previsto |
| Chiave pubblica non valida | Verificare che la chiave pubblica archiviata nel valore del registro di sistema PublicKey sia archiviata come [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| Dimensione della chiave pubblica non supportata | Attualmente sono supportate solo le chiavi RSA a 2048 bit. Configurare una chiave che soddisfi questo requisito |

Controllare anche se il valore `GuardedHost` in `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled` è impostato su un valore diverso da 0. Questa operazione Disabilita completamente i dump di arresto anomalo del sistema. In tal caso, impostarlo su 0.
