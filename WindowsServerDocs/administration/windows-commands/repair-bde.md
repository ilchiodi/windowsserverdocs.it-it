---
title: repair-bde
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 534dca1a-05f7-4ea8-ac24-4fe5f14f988a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 235640cacc6c0cca5ee9e820606082afe5d39d41
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820111"
---
# <a name="repair-bde"></a>repair-bde



Accede a dati su un disco rigido gravemente danneggiato crittografati se l'unità è stata crittografata con BitLocker. Repair-bde è in grado di ricostruire parti critiche dell'unità e di recuperare i dati ripristinabili, purché per decrittografare i dati venga utilizzata una password o una chiave di ripristino valida. Se i metadati di BitLocker sull'unità sono stati danneggiati, è necessario essere in grado di fornire un pacchetto di chiavi di backup oltre alla password o alla chiave di ripristino. Questo pacchetto di chiavi viene eseguito il backup in servizi di dominio Active Directory (AD DS) se è stata utilizzata l'impostazione predefinita per il backup di Active Directory. Con questo pacchetto di chiavi e la password o la chiave di ripristino è possibile decrittografare parti di un'unità protetta da BitLocker qualora il disco sia danneggiato. Ogni pacchetto di chiavi funzionerà solo per un'unità con l'identificatore di unità corrispondente. È possibile utilizzare il [Visualizzatore Password di ripristino BitLocker per Active Directory](https://technet.microsoft.com/library/dd875531(v=ws.10).aspx) per ottenere questo pacchetto di chiavi da AD DS.

> [!NOTE]
> Il Visualizzatore password di ripristino di BitLocker è incluso come una delle funzionalità di gestione facoltative installabili tramite Gestione server in Windows Server 2012.

Per lo strumento da riga di comando Repair-bde presenti le seguenti limitazioni:
-   Repair-bde non può ripristinare un'unità che non è riuscita durante il processo di crittografia o decrittografia.
-   Repair-bde presuppone che se l'unità con qualsiasi tipo di crittografia, quindi l'unità è stato completamente crittografato.



## <a name="syntax"></a>Sintassi

```
repair-bde <InputVolume> <OutputVolumeorImage> [-rk] [–rp] [-pw] [–kp] [–lf] [-f] [{-?|/?}]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> VolumeInput|Identifica la lettera di unità dell'unità crittografata con BitLocker che si desidera ripristinare. La lettera di unità deve includere un virgola; ad esempio: **c:**.|
|\<> VolumeOutputOImmagine|Identifica l'unità in cui archiviare il contenuto dell'unità riparato. Tutte le informazioni nell'unità di output verranno sovrascritto.|
|-rk|Identifica la posizione della chiave di ripristino che deve essere utilizzata per sbloccare il volume. Questo comando può essere specificato anche come **- recoverykey**.|
|-rp|Identifica la password di ripristino numerica che deve essere utilizzata per sbloccare il volume. Questo comando può anche essere specificato come **- recoverypassword**.|
|-pw|Identifica la password che deve essere utilizzata per sbloccare il volume. Questo comando può anche essere specificato come **-password**|
|-kp|Identifica il pacchetto di chiavi di ripristino che può essere utilizzato per sbloccare il volume. Questo comando può anche essere specificato come **- keypackage**.|
|-lf|Specifica il percorso del file che verranno archiviati Repair-bde errore, avviso e messaggi informativi. Questo comando può essere specificato anche come **- file di registro**.|
|-f|Forza un volume da smontare anche se non può essere bloccato. Questo comando può essere specificato anche come **-force**.|
|-? o /?|Visualizza la Guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

Se non viene specificato il percorso di un pacchetto di chiavi, **repair-bde** cercherà l'unità per un pacchetto di chiavi. Tuttavia, se il disco rigido è danneggiato, **repair-bde** potrebbe non essere in grado di trovare il pacchetto e verrà chiesto di fornire il percorso.

## <a name="examples"></a>Esempi

Per tentare di ripristinare l'unità C e scrivere il contenuto dall'unità C all'unità D usando il file della chiave di ripristino (RecoveryKey.. di) archiviato nell'unità F e scrive i risultati del tentativo nel file di log (log. txt) nell'unità Z.
```
repair-bde C: D: -rk F:\RecoveryKey.bek –lf Z:\log.txt
```
Per tentare di ripristinare l'unità C e scrivere il contenuto sull'unità C nell'unità D utilizzando la password di ripristino di 48 cifre specificata. La password di ripristino deve essere tipizzata in otto blocchi di sei cifre con un trattino separando ogni blocco.
```
repair-bde C: D: -rp 111111-222222-333333-444444-555555-666666-777777-888888
```
Per forzare lo smontaggio dell'unità C, quindi tenta di ripristinare l'unità C e scrivere il contenuto sull'unità C nell'unità D usando il pacchetto della chiave di ripristino e il file di chiave di ripristino (RecoveryKey... di ripristino) archiviati nell'unità F.
```
repair-bde C: D: -kp F:\RecoveryKeyPackage -rk F:\RecoveryKey.bek -f
```
Per tentare di ripristinare l'unità C e scrivere il contenuto dall'unità C all'unità D ed è necessario digitare una password per sbloccare l'unità C: quando richiesto:
```
repair-bde C: D: -pw
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)