---
title: Informazioni sulla crittografia del dump
description: Viene descritto come crittografare i file di dump e risolvere i problemi di crittografia.
ms.prod: windows-server
manager: dongill
ms.topic: article
author: larsiwer
ms.asset: b78ab493-e7c3-41f5-ab36-29397f086f32
ms.author: kathydav
ms.date: 11/03/2016
ms.openlocfilehash: a7df8de5a828b68a341191eaa1a400f80dd9127b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852904"
---
# <a name="about-dump-encryption"></a>Informazioni sulla crittografia del dump
La crittografia del dump può essere usata per crittografare i dump di arresto anomalo del sistema e i dump Live generati per un sistema. I dump vengono crittografati usando una chiave di crittografia simmetrica generata per ogni dump. Questa chiave viene quindi crittografata usando la chiave pubblica specificata dall'amministratore attendibile dell'host (protezione con chiave di crittografia del dump di arresto anomalo del sistema). In questo modo si garantisce che solo un utente con la chiave privata corrispondente possa decrittografare e quindi accedere al contenuto del dump. Questa funzionalità viene sfruttata in un'infrastruttura sorvegliata.
Nota: se si configura la crittografia del dump, disabilitare anche Segnalazione errori Windows. WER non può leggere i dump di arresto anomalo crittografati.

## <a name="configuring-dump-encryption"></a>Configurazione della crittografia dump
### <a name="manual-configuration"></a>Configurazione manuale
Per attivare la crittografia del dump utilizzando il registro di sistema, configurare i seguenti valori del registro di sistema in `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl`

| Nome valore | Type | Valore |
| ---------- | ---- | ----- |
| DumpEncryptionEnabled | DWORD | 1 per abilitare la crittografia del dump, 0 per disabilitare la crittografia del dump |
| EncryptionCertificates\Certificate.1::P ublicKey | Binary | Chiave pubblica (RSA, 2048 bit) da usare per la crittografia dei dump. Questo deve essere formattato come [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| EncryptionCertificates\Certificate.1:: identificazione personale | String | Identificazione personale del certificato per consentire la ricerca automatica della chiave privata nell'archivio certificati locale durante la decrittografia di un dump di arresto anomalo del sistema. |


### <a name="configuration-using-script"></a>Configurazione tramite script
Per semplificare la configurazione, è disponibile uno [script di esempio](https://github.com/Microsoft/Virtualization-Documentation/tree/live/hyperv-tools/DumpEncryption) per abilitare la crittografia del dump basata su una chiave pubblica di un certificato.

1. In un ambiente attendibile: creare un certificato con una chiave RSA a 2048 bit ed esportare il certificato pubblico
2. Negli host di destinazione: importare il certificato pubblico nell'archivio certificati locale
3. Eseguire lo script di configurazione di esempio 
    ```
    .\Set-DumpEncryptionConfiguration.ps1 -Certificate (Cert:\CurrentUser\My\093568AB328DF385544FAFD57EE53D73EFAAF519) -Force
    ```

## <a name="decrypting-encrypted-dumps"></a>Decrittografia di dump crittografati
Per decrittografare un file di dump crittografato esistente, è necessario scaricare e installare gli strumenti di debug per Windows. Questo set di strumenti contiene KernelDumpDecrypt. exe, che può essere usato per decrittografare un file di dump crittografato.
Se il certificato che include la chiave privata è presente nell'archivio certificati dell'utente corrente, è possibile decrittografare il file di dump chiamando

```
    KernelDumpDecrypt.exe memory.dmp memory_decr.dmp
```
Dopo la decrittografia, gli strumenti come WinDbg possono aprire il file dump decrittografato.

## <a name="troubleshooting-dump-encryption"></a>Risoluzione dei problemi di crittografia dump
Se la crittografia del dump è abilitata in un sistema ma non viene generato alcun dump, controllare il registro eventi del sistema `System` per `Kernel-IO` evento 1207. Quando non è possibile inizializzare la crittografia del dump, questo evento viene creato e i dump sono disabilitati.

| Messaggio di errore dettagliato | Passaggi per attenuare |
| ---------------------- | ----------------- |
| Registro di sistema di chiave pubblica o identificazione personale mancante | Controllare se i valori del registro di sistema sono presenti nel percorso previsto |
| Chiave pubblica non valida | Verificare che la chiave pubblica archiviata nel valore del registro di sistema PublicKey sia archiviata come [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| Dimensione della chiave pubblica non supportata | Attualmente sono supportate solo le chiavi RSA a 2048 bit. Configurare una chiave che soddisfi questo requisito |

Verificare inoltre che il valore `GuardedHost` in `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled` sia impostato su un valore diverso da 0. Questa operazione Disabilita completamente i dump di arresto anomalo del sistema. In tal caso, impostarlo su 0.
