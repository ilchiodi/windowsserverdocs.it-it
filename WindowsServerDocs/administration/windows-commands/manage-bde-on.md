---
title: Manage-bde on
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f6a12814-df74-416c-a04a-62ea8512263e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 471bea3946aff39689ad219585d10c2d43f99a93
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820651"
---
# <a name="manage-bde-on"></a>Manage-bde: on



Crittografa l'unità e consente di attivare BitLocker.

## <a name="syntax"></a>Sintassi

```
manage-bde –on <Drive> {[-recoveryPassword <NumericalPassword>]|[-recoverykey <PathToExternalDirectory>]|[-startupkey <PathToExternalKeyDirectory>]|[-certificate]|
[-tpmandpin]|[-tpmandpinandstartupkey <PathToExternalKeyDirectory>]|[-tpmandstartupkey <PathToExternalKeyDirectory>]|[-password]|[-ADAccountOrGroup <Domain\Account>]}
[-UsedSpaceOnly][-encryptionmethod {aes128_diffuser|aes256_diffuser|aes128|aes256}] [-skiphardwaretest] [-discoveryvolumetype <FileSystemType>] [-ForceEncryptionType <type>] [-RemoveVolumeShadowCopies][-computername <Name>]
[{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> unità|Rappresenta una lettera di unità seguita da due punti.|
|-recoverypassword|Aggiunge una protezione con password numerica. È inoltre possibile utilizzare **- rp** come una versione abbreviata di questo comando.|
|\<> NumericalPassword|Rappresenta la password di ripristino.|
|-recoverykey|Aggiunge una protezione con chiave esterna per il ripristino. È inoltre possibile utilizzare **- rk** come una versione abbreviata di questo comando.|
|\<> PathToExternalDirectory|Rappresenta il percorso della directory per la chiave di ripristino.|
|-chiave di avvio|Aggiunge una protezione con chiave esterna per l'avvio. È inoltre possibile utilizzare **-sk** come una versione abbreviata di questo comando.|
|\<> PathToExternalKeyDirectory|Rappresenta il percorso della directory per la chiave di avvio.|
|-certificato|Aggiunge una protezione con chiave pubblica per un'unità di dati. È inoltre possibile utilizzare **-cert** come una versione abbreviata di questo comando.|
|-tpmandpin|Aggiunge un modulo TPM (Trusted Platform) e protezione (PIN) numero di identificazione personale per l'unità del sistema operativo. È inoltre possibile utilizzare **- tp** come una versione abbreviata di questo comando.|
|-tpmandstartupkey|Aggiunge una protezione con chiave TPM e avvio per l'unità del sistema operativo. È inoltre possibile utilizzare **-tsk** come una versione abbreviata di questo comando.|
|-tpmandpinandstartupkey|Aggiunge un TPM, PIN e protezione con chiave di avvio per l'unità del sistema operativo. È inoltre possibile utilizzare **- tpsk** come una versione abbreviata di questo comando.|
|-password|Aggiunge una protezione con chiave password per l'unità dati. È inoltre possibile utilizzare **- pw** come una versione abbreviata di questo comando.|
|-ADAccountOrGroup|Aggiunge una protezione con l'identità basata su SID per il volume. Il volume verrà sbloccato automaticamente se l'utente o il computer dispone delle credenziali corrette. Quando si specifica un account computer, aggiungere un oggetto **$** al nome del computer e specificare **– Service** per indicare che lo sblocco deve verificarsi nel contenuto del server BitLocker anziché dell'utente. È inoltre possibile utilizzare **-sid** come una versione abbreviata di questo comando.|
|-UsedSpaceOnly|Imposta la modalità di crittografia per la crittografia solo dello spazio utilizzato. Le sezioni del volume che contiene lo spazio utilizzato verranno crittografate, ma non lo spazio disponibile. Se questa opzione non è specificata, tutti spazio utilizzato e spazio libero nel volume verrà crittografato. È inoltre possibile utilizzare **-utilizzato** come una versione abbreviata di questo comando.|
|-encryptionMethod|Consente di configurare le dimensioni di chiave e l'algoritmo di crittografia. È inoltre possibile utilizzare **-em** come una versione abbreviata di questo comando.|
|-skiphardwaretest|Inizia la crittografia senza un test dell'hardware. È inoltre possibile utilizzare **-s** come una versione abbreviata di questo comando.|
|-discoveryvolumetype|Specifica il file system da utilizzare per l'unità di dati di individuazione. L'unità di dati di individuazione è un'unità nascosta aggiunta a un'unità dati rimovibili formattate FAT, protetto da BitLocker che contiene il lettore BitLocker To Go in modo che i sistemi operativi Windows Vista o Windows XP può essere utilizzati per visualizzare le unità protette con BitLocker.|
|-ForceEncryptionType|Forza utilizzare software o hardware di crittografia di BitLocker. È possibile specificare **Hardware** o **Software** come il tipo di crittografia. Se il **hardware** viene selezionato un parametro, ma l'unità non supporta la crittografia hardware, gestire-bde restituisce un errore. Se le impostazioni di criteri di gruppo impedisce il tipo di crittografia, gestire-bde restituisce un errore. È inoltre possibile utilizzare **- FET con** come una versione abbreviata di questo comando.|
|-RemoveVolumeShadowCopies|Forzare deletikon di copie Shadow del Volume per il volume. Non sarà in grado di ripristinare questo volume utilizzando punti di ripristino precedenti del sistema dopo aver eseguito questo comando. È inoltre possibile utilizzare **- rvsc** come una versione abbreviata di questo comando.|
|\<> FileSystemType|Specifica il file System possono essere utilizzati con le unità di dati di individuazione: FAT32, predefinito o nessuno.|
|-computername|Specifica che Manage-bde viene utilizzata per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.|
|\<Name>|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?|Visualizza una breve guida al prompt dei comandi.|
|-Help o-h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="examples"></a>Esempi

Per illustrare l'uso del comando **-on** per attivare BitLocker per l'unità C e aggiungere una password di ripristino all'unità.
```
manage-bde –on C: -recoverypassword
```
Per illustrare l'uso del comando **-on** per attivare BitLocker per l'unità C, aggiungere una password di ripristino all'unità e salvare una chiave di ripristino nell'unità E.
```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```
Per illustrare l'uso del comando **-on** per attivare BitLocker per l'unità C usando una protezione con chiave esterna, ad esempio una chiave USB, per sbloccare l'unità del sistema operativo. Questo metodo è necessario se si utilizza BitLocker con i computer che non dispone di un TPM.
```
manage-bde -on C: -startupkey E:\
```
Per illustrare l'uso del comando **-on** per attivare BitLocker per l'unità dati e e aggiungere una protezione con chiave della password. Gestire-bde verrà richiesto di immettere la password dopo l'immissione del comando.
```
manage-bde –on E: -pw
```
Per illustrare l'utilizzo del comando **-on** per attivare BitLocker per l'unità del sistema operativo C e utilizzare la crittografia basata su hardware.
```
manage-bde –on C: -fet Hardware
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)