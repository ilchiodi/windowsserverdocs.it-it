---
title: Manage-bde on
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6a12814-df74-416c-a04a-62ea8512263e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a95bbc375c0a5b62b96f7c68f7d5ab5e09371d1c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373996"
---
# <a name="manage-bde-on"></a>Manage-bde: on



Crittografa l'unità e consente di attivare BitLocker. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
manage-bde –on <Drive> {[-recoveryPassword <NumericalPassword>]|[-recoverykey <PathToExternalDirectory>]|[-startupkey <PathToExternalKeyDirectory>]|[-certificate]|
[-tpmandpin]|[-tpmandpinandstartupkey <PathToExternalKeyDirectory>]|[-tpmandstartupkey <PathToExternalKeyDirectory>]|[-password]|[-ADAccountOrGroup <Domain\Account>]}
[-UsedSpaceOnly][-encryptionmethod {aes128_diffuser|aes256_diffuser|aes128|aes256}] [-skiphardwaretest] [-discoveryvolumetype <FileSystemType>] [-ForceEncryptionType <type>] [-RemoveVolumeShadowCopies][-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Drive >|Rappresenta una lettera di unità seguita da due punti.|
|-recoverypassword|Aggiunge una protezione con password numerica. È inoltre possibile utilizzare **- rp** come una versione abbreviata di questo comando.|
|\<NumericalPassword >|Rappresenta la password di ripristino.|
|-recoverykey|Aggiunge una protezione con chiave esterna per il ripristino. È inoltre possibile utilizzare **- rk** come una versione abbreviata di questo comando.|
|\<PathToExternalDirectory >|Rappresenta il percorso della directory per la chiave di ripristino.|
|-chiave di avvio|Aggiunge una protezione con chiave esterna per l'avvio. È inoltre possibile utilizzare **-sk** come una versione abbreviata di questo comando.|
|\<PathToExternalKeyDirectory >|Rappresenta il percorso della directory per la chiave di avvio.|
|-certificato|Aggiunge una protezione con chiave pubblica per un'unità di dati. È inoltre possibile utilizzare **-cert** come una versione abbreviata di questo comando.|
|-tpmandpin|Aggiunge un modulo TPM (Trusted Platform) e protezione (PIN) numero di identificazione personale per l'unità del sistema operativo. È inoltre possibile utilizzare **- tp** come una versione abbreviata di questo comando.|
|-tpmandstartupkey|Aggiunge una protezione con chiave TPM e avvio per l'unità del sistema operativo. È inoltre possibile utilizzare **-tsk** come una versione abbreviata di questo comando.|
|-tpmandpinandstartupkey|Aggiunge un TPM, PIN e protezione con chiave di avvio per l'unità del sistema operativo. È inoltre possibile utilizzare **- tpsk** come una versione abbreviata di questo comando.|
|-password|Aggiunge una protezione con chiave password per l'unità dati. È inoltre possibile utilizzare **- pw** come una versione abbreviata di questo comando.|
|-ADAccountOrGroup|Aggiunge una protezione con l'identità basata su SID per il volume. Il volume verrà sbloccato automaticamente se l'utente o il computer dispone delle credenziali corrette. Quando si specifica un account computer, aggiungere un **$** nel computer di nome e specificare **– servizio** per indicare che lo sblocco devono essere eseguite nel contenuto del server di BitLocker anziché sull'utente. È inoltre possibile utilizzare **-sid** come una versione abbreviata di questo comando.|
|-UsedSpaceOnly|Imposta la modalità di crittografia per la crittografia solo dello spazio utilizzato. Le sezioni del volume che contiene lo spazio utilizzato verranno crittografate, ma non lo spazio disponibile. Se questa opzione non è specificata, tutti spazio utilizzato e spazio libero nel volume verrà crittografato. È inoltre possibile utilizzare **-utilizzato** come una versione abbreviata di questo comando.|
|-encryptionMethod|Consente di configurare le dimensioni di chiave e l'algoritmo di crittografia. È inoltre possibile utilizzare **-em** come una versione abbreviata di questo comando.|
|-skiphardwaretest|Inizia la crittografia senza un test dell'hardware. È inoltre possibile utilizzare **-s** come una versione abbreviata di questo comando.|
|-discoveryvolumetype|Specifica il file system da utilizzare per l'unità di dati di individuazione. L'unità di dati di individuazione è un'unità nascosta aggiunta a un'unità dati rimovibili formattate FAT, protetto da BitLocker che contiene il lettore BitLocker To Go in modo che i sistemi operativi Windows Vista o Windows XP può essere utilizzati per visualizzare le unità protette con BitLocker.|
|-ForceEncryptionType|Forza utilizzare software o hardware di crittografia di BitLocker. È possibile specificare **Hardware** o **Software** come il tipo di crittografia. Se il **hardware** viene selezionato un parametro, ma l'unità non supporta la crittografia hardware, gestire-bde restituisce un errore. Se le impostazioni di criteri di gruppo impedisce il tipo di crittografia, gestire-bde restituisce un errore. È inoltre possibile utilizzare **- FET con** come una versione abbreviata di questo comando.|
|-RemoveVolumeShadowCopies|Forzare deletikon di copie Shadow del Volume per il volume. Non sarà in grado di ripristinare questo volume utilizzando punti di ripristino precedenti del sistema dopo aver eseguito questo comando. È inoltre possibile utilizzare **- rvsc** come una versione abbreviata di questo comando.|
|\<FileSystemType >|Specifica i file System che è possibile usare con le unità dati di individuazione: FAT32, default o None.|
|-computername|Specifica che Manage-bde viene utilizzata per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.|
|\<Nome >|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?|Visualizza una breve guida al prompt dei comandi.|
|-Help o-h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo di **-su** comando per attivare BitLocker per l'unità C e aggiungere una password di ripristino nell'unità.
```
manage-bde –on C: -recoverypassword
```
Nell'esempio seguente viene illustrato l'utilizzo di **-su** comando per attivare BitLocker per l'unità C, aggiungere una password di ripristino per l'unità e salvare una chiave di ripristino unità E.
```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```
Nell'esempio seguente viene illustrato l'utilizzo di **-su** comando per attivare BitLocker per l'unità C con una protezione con chiave esterna (ad esempio una chiave USB) per sbloccare l'unità del sistema operativo. Questo metodo è necessario se si utilizza BitLocker con i computer che non dispone di un TPM.
```
manage-bde -on C: -startupkey E:\
```
Nell'esempio seguente viene illustrato l'utilizzo di **-su** comando per attivare BitLocker per unità di dati E di aggiungere una protezione con chiave password. Gestire-bde verrà richiesto di immettere la password dopo l'immissione del comando.
```
manage-bde –on E: -pw
```
Nell'esempio seguente viene illustrato l'utilizzo di **-su** comando per attivare BitLocker per unità del sistema operativo C e utilizzare la crittografia basata su hardware.
```
manage-bde –on C: -fet Hardware
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)