---
title: repair-bde
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 534dca1a-05f7-4ea8-ac24-4fe5f14f988a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 817e5fb5cf032376ddfddb3a54f73411ac175def
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384546"
---
# <a name="repair-bde"></a>repair-bde



Accede a dati su un disco rigido gravemente danneggiato crittografati se l'unità è stata crittografata con BitLocker. Repair-bde è in grado di ricostruire parti critiche dell'unità e di recuperare i dati ripristinabili, purché per decrittografare i dati venga utilizzata una password o una chiave di ripristino valida. Se i metadati di BitLocker sull'unità sono stati danneggiati, è necessario essere in grado di fornire un pacchetto di chiavi di backup oltre alla password o alla chiave di ripristino. Questo pacchetto di chiavi viene eseguito il backup in servizi di dominio Active Directory (AD DS) se è stata utilizzata l'impostazione predefinita per il backup di Active Directory. Con questo pacchetto di chiavi e la password o la chiave di ripristino è possibile decrittografare parti di un'unità protetta da BitLocker qualora il disco sia danneggiato. Ogni pacchetto di chiavi funzionerà solo per un'unità con l'identificatore di unità corrispondente. È possibile utilizzare il [Visualizzatore Password di ripristino BitLocker per Active Directory](https://technet.microsoft.com/library/dd875531(v=ws.10).aspx) per ottenere questo pacchetto di chiavi da AD DS.

> [!NOTE]
> Il Visualizzatore password di ripristino di BitLocker è incluso come una delle funzionalità di gestione facoltative installabili tramite Gestione server in Windows Server 2012.

Per lo strumento da riga di comando Repair-bde presenti le seguenti limitazioni:
-   Repair-bde non può ripristinare un'unità che non è riuscita durante il processo di crittografia o decrittografia.
-   Repair-bde presuppone che se l'unità con qualsiasi tipo di crittografia, quindi l'unità è stato completamente crittografato.

Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
repair-bde <InputVolume> <OutputVolumeorImage> [-rk] [–rp] [-pw] [–kp] [–lf] [-f] [{-?|/?}]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<InputVolume >|Identifica la lettera di unità dell'unità crittografata con BitLocker che si desidera ripristinare. La lettera di unità deve includere i due punti; Per esempio: **C:** .|
|\<OutputVolumeorImage >|Identifica l'unità in cui archiviare il contenuto dell'unità riparato. Tutte le informazioni nell'unità di output verranno sovrascritto.|
|-rk|Identifica la posizione della chiave di ripristino che deve essere utilizzata per sbloccare il volume. Questo comando può essere specificato anche come **- recoverykey**.|
|-rp|Identifica la password di ripristino numerica che deve essere utilizzata per sbloccare il volume. Questo comando può anche essere specificato come **- recoverypassword**.|
|-pw|Identifica la password che deve essere utilizzata per sbloccare il volume. Questo comando può anche essere specificato come **-password**|
|-kp|Identifica il pacchetto di chiavi di ripristino che può essere utilizzato per sbloccare il volume. Questo comando può anche essere specificato come **- keypackage**.|
|-lf|Specifica il percorso del file che verranno archiviati Repair-bde errore, avviso e messaggi informativi. Questo comando può essere specificato anche come **- file di registro**.|
|-f|Forza un volume da smontare anche se non può essere bloccato. Questo comando può essere specificato anche come **-force**.|
|-? o /?|Visualizza la Guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Se non viene specificato il percorso di un pacchetto di chiavi, **repair-bde** cercherà l'unità per un pacchetto di chiavi. Tuttavia, se il disco rigido è danneggiato, **repair-bde** potrebbe non essere in grado di trovare il pacchetto e verrà chiesto di fornire il percorso.

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente tenta di ripristinare l'unità C e scrivere il contenuto dall'unità C nell'unità D utilizzando il file chiave ripristino (RecoveryKey.bek) archiviato nell'unità F e scrive i risultati di questo tentativo per il file di log (txt) in unità Z.
```
repair-bde C: D: -rk F:\RecoveryKey.bek –lf Z:\log.txt
```
Nell'esempio seguente tenta di ripristinare l'unità C e scrivere il contenuto sull'unità C nell'unità D utilizzando la password di ripristino di 48 cifre specificata. La password di ripristino deve essere tipizzata in otto blocchi di sei cifre con un trattino separando ogni blocco.
```
repair-bde C: D: -rp 111111-222222-333333-444444-555555-666666-777777-888888
```
Nell'esempio seguente forza smontaggio dell'unità C e quindi tenta di ripristinare l'unità C e scrivere il contenuto sull'unità C nell'unità D con il pacchetto della chiave di ripristino e ripristino file di chiave (RecoveryKey.bek) archiviate nell'unità F.
```
repair-bde C: D: -kp F:\RecoveryKeyPackage -rk F:\RecoveryKey.bek -f
```
Nell'esempio seguente tenta di ripristinare l'unità C e scrivere il contenuto dall'unità C nell'unità D ed è necessario digitare una password per sbloccare l'unità c quando richiesto:
```
repair-bde C: D: -pw
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)