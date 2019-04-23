---
title: diskcopy
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fd21efa-52cc-4e70-a7fe-35125a435106
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/07/2018
ms.openlocfilehash: 5b9343dc2f6b4c74da5a9d89a2ea804b702248cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841172"
---
# <a name="diskcopy"></a>diskcopy



Copia il contenuto del disco floppy nell'unità di origine in un disco floppy formattato o nell'unità di destinazione. Se utilizzata senza parametri, **verrà** usi l'unità corrente per il disco di origine e il disco di destinazione.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

> [!NOTE]
> Questo comando non è incluso in Windows 10.

## <a name="syntax"></a>Sintassi

```
diskcopy [<Drive1>: [<Drive2>:]] [/v]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Drive1>|Specifica l'unità che contiene il disco di origine.|
|\<Drive2>|Specifica l'unità che contiene il disco di destinazione.|
|/v|Verifica che le informazioni vengano copiate correttamente. Questa opzione rallenta il processo di copia.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Uso di dischi

    **Verrà** funziona solo con un disco rimovibile, ad esempio dischi floppy, che deve essere dello stesso tipo. Non è possibile usare **verrà** con un disco rigido. Se si specifica un'unità disco rigido per *unità1* oppure *unità2*, **verrà** Visualizza il messaggio di errore seguente:  
    ```
    Invalid drive specification
    Specified drive does not exist or is nonremovable
    ```  
    Il **verrà** comando richiede di inserire l'origine e destinazione di dischi e attende che è possibile premere un tasto sulla tastiera prima di continuare.

    Al termine della copia del disco, **verrà** viene visualizzato il messaggio seguente:  
    ```
    Copy another diskette (Y/N)?
    ```  
    Se si preme Y **verrà** chiede all'utente di inserire i dischi di origine e destinazione per l'operazione di copia successiva. Per arrestare il **verrà** elaborare, premere **N**.

    Se si desidera copiare su un disco floppy formattato in *unità2*, **verrà** formatta il disco con lo stesso numero di lati e settori per traccia presenti sul disco nel *unità1*. **Verrà** mentre formatta il disco e copia i file visualizzato il messaggio seguente:  
    ```
    Formatting while copying
    ```  
-   Numeri di serie disco

    Se il disco di origine ha un numero di serie volume, **verrà** crea un nuovo numero di serie volume per il disco di destinazione e visualizza il numero, una volta completata l'operazione di copia.
-   Omettendo i parametri di unità

    Se si omette il *unità2* parametro **verrà** Usa l'unità corrente come unità di destinazione. Se si omettono entrambi i parametri, unità **verrà** usi l'unità corrente per entrambi. Se l'unità corrente è identico *unità1*, **verrà** chiederà di scambiare dischi in base alle esigenze.
-   Usando un'unità per la copia

    Eseguire **verrà** da un'unità diversa dall'unità disco floppy, ad esempio C unità. Se un disco floppy *unità1* e un disco floppy *unità2* sono uguali, **verrà** viene richiesto di cambiare i dischi. Se i dischi contengono più informazioni rispetto a può contenere la memoria disponibile, **verrà** non è possibile leggere tutte le informazioni in una sola volta. **Verrà** legge dal disco di origine, scrive sul disco di destinazione e viene richiesto di inserire di nuovo il disco di origine. Questo processo continua fino a quando non è stato copiato l'intero disco.
-   Come evitare la frammentazione del disco

    La frammentazione è la presenza di aree di piccole dimensioni di spazio su disco inutilizzato tra i file esistenti in un disco. Un disco di origine frammentato può rallentare il processo di individuazione, la lettura o scrittura di file.

    In quanto **verrà** rende una copia esatta del disco di origine sul disco di destinazione, qualsiasi frammentazione del disco di origine verrà trasferita sul disco di destinazione. Per evitare il trasferimento di frammentazione da un disco a un altro, usare **copia** oppure **xcopy** copiare il disco. In quanto **copia** e **xcopy** copia i file in sequenza, il nuovo disco non è frammentato.

> [!NOTE]
> Non è possibile usare **xcopy** possa copiare un disco di avvio.
-   La comprensione **verrà** codici di uscita

    Nella tabella seguente viene illustrato ogni codice di uscita.  
    |Codice di uscita|Descrizione|
    |---------|-----------|
    |0|Operazione di copia è stata completata|
    |1|Si è verificato un errore di lettura/scrittura non irreversibile|
    |3|Si è verificato un errore hardware irreversibile|
    |4|Si è verificato un errore di inizializzazione|

    Per elaborare i codici di uscita restituiti da **verrà**, è possibile utilizzare il *ERRORLEVEL* variabile di ambiente nella **se** riga di comando in un file batch.

## <a name="BKMK_examples"></a>Esempi

Per copiare il disco nell'unità B. il disco nell'unità, digitare:
```
diskcopy b: a:
```
Per usare un'unità disco floppy per copiare un disco floppy a altra, passare all'unità C e quindi digitare:

r: r: dell'operazione

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)