---
title: Manage-bde on
description: Argomento di riferimento per il comando Manage-bde on, che consente di crittografare l'unità e di accendere BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f6a12814-df74-416c-a04a-62ea8512263e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bad56c9ade19d94fc5acc9b34a8cc42fe43b7c0d
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222853"
---
# <a name="manage-bde-on"></a>Manage-bde on

Crittografa l'unità e consente di attivare BitLocker.

## <a name="syntax"></a>Sintassi

```
manage-bde –on <drive> {[-recoverypassword <numericalpassword>]|[-recoverykey <pathtoexternaldirectory>]|[-startupkey <pathtoexternalkeydirectory>]|[-certificate]|
[-tpmandpin]|[-tpmandpinandstartupkey <pathtoexternalkeydirectory>]|[-tpmandstartupkey <pathtoexternalkeydirectory>]|[-password]|[-ADaccountorgroup <domain\account>]}
[-usedspaceonly][-encryptionmethod {aes128_diffuser|aes256_diffuser|aes128|aes256}] [-skiphardwaretest] [-discoveryvolumetype <filesystemtype>] [-forceencryptiontype <type>] [-removevolumeshadowcopies][-computername <name>]
[{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<drive>` | Rappresenta una lettera di unità seguita da due punti. |
| -recoverypassword | Aggiunge una protezione con password numerica. È inoltre possibile utilizzare **- rp** come una versione abbreviata di questo comando. |
| `<numericalpassword>` | Rappresenta la password di ripristino. |
| -recoverykey | Aggiunge una protezione con chiave esterna per il ripristino. È inoltre possibile utilizzare **- rk** come una versione abbreviata di questo comando. |
| `<pathtoexternaldirectory>` | Rappresenta il percorso della directory per la chiave di ripristino. |
| -chiave di avvio | Aggiunge una protezione con chiave esterna per l'avvio. È inoltre possibile utilizzare **-sk** come una versione abbreviata di questo comando. |
| `<pathtoexternalkeydirectory>` | Rappresenta il percorso della directory per la chiave di avvio. |
| -certificato | Aggiunge una protezione con chiave pubblica per un'unità di dati. È inoltre possibile utilizzare **-cert** come una versione abbreviata di questo comando. |
| -tpmandpin | Aggiunge un modulo TPM (Trusted Platform) e protezione (PIN) numero di identificazione personale per l'unità del sistema operativo. È inoltre possibile utilizzare **- tp** come una versione abbreviata di questo comando. |
| -tpmandstartupkey | Aggiunge una protezione con chiave TPM e avvio per l'unità del sistema operativo. È inoltre possibile utilizzare **-tsk** come una versione abbreviata di questo comando. |
| -tpmandpinandstartupkey | Aggiunge un TPM, PIN e protezione con chiave di avvio per l'unità del sistema operativo. È inoltre possibile utilizzare **- tpsk** come una versione abbreviata di questo comando. |
| -password | Aggiunge una protezione con chiave password per l'unità dati. È inoltre possibile utilizzare **- pw** come una versione abbreviata di questo comando. |
| -ADaccountorgroup | Aggiunge una protezione con l'identità basata su SID per il volume. Il volume verrà sbloccato automaticamente se l'utente o il computer dispone delle credenziali corrette. Quando si specifica un account computer, aggiungere un oggetto **$** al nome del computer e specificare **– Service** per indicare che lo sblocco deve verificarsi nel contenuto del server BitLocker anziché dell'utente. È inoltre possibile utilizzare **-sid** come una versione abbreviata di questo comando. |
| -usedspaceonly | Imposta la modalità di crittografia per la crittografia solo dello spazio utilizzato. Le sezioni del volume che contiene lo spazio utilizzato verranno crittografate, ma non lo spazio disponibile. Se questa opzione non è specificata, tutto lo spazio utilizzato e lo spazio disponibile nel volume verranno crittografati. È inoltre possibile utilizzare **-utilizzato** come una versione abbreviata di questo comando. |
| -encryptionMethod | Consente di configurare le dimensioni di chiave e l'algoritmo di crittografia. È inoltre possibile utilizzare **-em** come una versione abbreviata di questo comando. |
| -skiphardwaretest | Inizia la crittografia senza un test dell'hardware. È inoltre possibile utilizzare **-s** come una versione abbreviata di questo comando. |
| -discoveryvolumetype | Specifica il file system da utilizzare per l'unità di dati di individuazione. L'unità dati di individuazione è un'unità nascosta aggiunta a un'unità dati rimovibile con formattazione FAT e protetta da BitLocker che contiene il lettore BitLocker To Go. |
| -forceencryptiontype | Forza utilizzare software o hardware di crittografia di BitLocker. È possibile specificare **Hardware** o **Software** come il tipo di crittografia. Se il parametro **hardware** è selezionato, ma l'unità non supporta la crittografia hardware, Manage-bde restituisce un errore. Se le impostazioni di criteri di gruppo impedisce il tipo di crittografia, gestire-bde restituisce un errore. È inoltre possibile utilizzare **- FET con** come una versione abbreviata di questo comando. |
| -removevolumeshadowcopies | Forza l'eliminazione delle copie shadow del volume per il volume. Non sarà possibile ripristinare questo volume usando i punti di ripristino del sistema precedenti dopo l'esecuzione di questo comando. È inoltre possibile utilizzare **- rvsc** come una versione abbreviata di questo comando. |
| `<filesystemtype>` | Specifica il file System possono essere utilizzati con le unità di dati di individuazione: FAT32, predefinito o nessuno. |
| -computername | Specifica che manage-bde viene utilizzato per modificare la protezione BitLocker in un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando. |
| `<name>` | Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer. |
| -? o /? | Visualizza una breve guida al prompt dei comandi. |
| -Help o-h | Visualizza la Guida completa al prompt dei comandi. |

### <a name="examples"></a>Esempi

Per attivare BitLocker per l'unità C e aggiungere una password di ripristino all'unità, digitare:

```
manage-bde –on C: -recoverypassword
```

Per attivare BitLocker per l'unità C, aggiungere una password di ripristino all'unità e per salvare una chiave di ripristino nell'unità E, digitare:

```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```

Per attivare BitLocker per l'unità C, usando una protezione con chiave esterna, ad esempio una chiave USB, per sbloccare l'unità del sistema operativo, digitare:

```
manage-bde -on C: -startupkey E:\
```

> [!IMPORTANT]
> Questo metodo è obbligatorio se si utilizza BitLocker con computer che non dispongono di un TPM.

Per attivare BitLocker per l'unità dati E e per aggiungere una protezione con chiave password, digitare:

```
manage-bde –on E: -pw
```

Per attivare BitLocker per l'unità del sistema operativo C e per utilizzare la crittografia basata su hardware, digitare:

```
manage-bde –on C: -fet hardware
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Manage-bde off](manage-bde-off.md)

- [comando di sospensione Manage-bde](manage-bde-pause.md)

- [comando Manage-bde resume](manage-bde-resume.md)

- [comando Manage-bde](manage-bde.md)
