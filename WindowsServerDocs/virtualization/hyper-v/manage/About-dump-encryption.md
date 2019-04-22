---
title: Sulla crittografia del dump
description: Viene descritto come crittografare i file di dump e risolvere i problemi di crittografia.
ms.prod: windows-server-threshold
manager: dongill
ms.topic: article
author: larsiwer
ms.asset: b78ab493-e7c3-41f5-ab36-29397f086f32
ms.author: kathydav
ms.date: 11/03/2016
ms.openlocfilehash: e0ca8829aa8f93e7543b4183539beade3b4aeb47
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812392"
---
# <a name="about-dump-encryption"></a>Sulla crittografia del dump
Crittografia dump può essere usata per crittografare i dump di arresto anomalo del sistema e in tempo reale dump generati per un sistema. I dump vengono crittografati usando una chiave di crittografia simmetrica che viene generata per ogni immagine. Questa chiave viene quindi crittografata usando la chiave pubblica specificata dall'amministratore attendibile dell'host (arresto anomalo del sistema dump crittografia protezione con chiave). Ciò garantisce che solo gli utenti che la corrispondente chiave privata può decrittografare e pertanto accedere a contenuto del dump. Questa funzionalità viene usata in un'infrastruttura sorvegliata.
Nota: Se si configura crittografia del dump, anche disabilitare segnalazione errori Windows. Segnalazione errori Windows non è in grado di leggere i dump di arresto anomalo del sistema crittografate.

# <a name="configuring-dump-encryption"></a>Configurazione di crittografia del dump
## <a name="manual-configuration"></a>Configurazione manuale
Per abilitare crittografia del dump usando il Registro di sistema, configurare i seguenti valori del Registro di sistema in `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl`

| Nome valore | Tipo | Value |
| ---------- | ---- | ----- |
| DumpEncryptionEnabled | DWORD | 1 per abilitare la crittografia del dump, 0 per disabilitare la crittografia del dump |
| EncryptionCertificates\Certificate.1::PublicKey | Binary | Chiave pubblica (RSA a 2048 Bit) che deve essere utilizzato per la crittografia dei dump. Questa operazione deve essere formattato come [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| EncryptionCertificates\Certificate.1::Thumbprint | Stringa | Identificazione personale del certificato per consentire la ricerca automatica della chiave privata nell'archivio certificati locale durante la decrittografia di un dump di arresto anomalo del sistema. |


## <a name="configuration-using-script"></a>Configurazione tramite script
Per semplificare la configurazione, un [script di esempio](https://github.com/Microsoft/Virtualization-Documentation/tree/live/hyperv-tools/DumpEncryption) è disponibile per abilitare crittografia del dump basata su una chiave pubblica da un certificato.

1. In un ambiente attendibile: Creare un certificato con una chiave RSA a 2048 Bit ed esportare il certificato pubblico
2. Negli host di destinazione: Importare il certificato pubblico nell'archivio certificati locale
3. Eseguire lo script di configurazione di esempio 
    ```
    .\Set-DumpEncryptionConfiguration.ps1 -Certificate (Cert:\CurrentUser\My\093568AB328DF385544FAFD57EE53D73EFAAF519) -Force
    ```

# <a name="decrypting-encrypted-dumps"></a>La decrittografia dei dump crittografato
Per decrittografare un file di dump crittografato esistente, è necessario scaricare e installare il debug di strumenti per Windows. Questo set di strumenti contiene KernelDumpDecrypt.exe che può essere utilizzato per decrittografare un file di dump crittografato.
Se il certificato con la chiave privata è presente nell'archivio certificati dell'utente corrente, il file di dump può essere decrittografato mediante la chiamata

```
    KernelDumpDecrypt.exe memory.dmp memory_decr.dmp
```
Dopo la decrittografia, strumenti quali WinDbg possono aprire il file di dump decrittografata.

# <a name="troubleshooting-dump-encryption"></a>Risoluzione dei problemi di crittografia del dump
Se crittografia del dump è abilitata in un sistema, ma non i dump vengono generati, controllare il sistema `System` registro eventi per `Kernel-IO` evento 1207. Quando non è possibile inizializzare crittografia del dump, questo evento viene creato e i dump vengono disabilitati.

| Messaggio di errore dettagliato | Procedure per attenuare |
| ---------------------- | ----------------- |
| Pubblica chiavi o un'identificazione personale del Registro di sistema mancanti | Controllare se sono presenti entrambi i valori del Registro di sistema nel percorso previsto |
| Chiave pubblica non valida | Assicurarsi che la chiave pubblica archiviata nel valore del Registro di sistema di chiave pubblica viene archiviata come [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| Dimensioni della chiave pubblica non supportata | Attualmente, sono supportate solo le chiavi RSA a 2048 Bit. Configurare una chiave che corrisponde a questo requisito |

Controllare anche se il valore `GuardedHost` sotto `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled` è impostata su un valore diverso da 0. Ciò Disabilita completamente i dump di arresto anomalo del sistema. In questo caso, impostarlo su 0.
