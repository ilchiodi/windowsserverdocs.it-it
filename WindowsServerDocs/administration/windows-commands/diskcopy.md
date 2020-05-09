---
title: diskcopy
description: Argomento di riferimento per il comando DISKCOPY, che copia il contenuto del disco floppy nell'unità di origine in un disco floppy formattato o non formattato nell'unità di destinazione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fd21efa-52cc-4e70-a7fe-35125a435106
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/07/2018
ms.openlocfilehash: b8fd482d7c2e5933f269320df2bff75f65195bc2
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992530"
---
# <a name="diskcopy"></a>diskcopy

Copia il contenuto del disco floppy nell'unità di origine in un disco floppy formattato o non formattato nell'unità di destinazione. Se utilizzata senza parametri, **diskcopy** utilizza l'unità corrente per il disco di origine e il disco di destinazione.

## <a name="syntax"></a>Sintassi

```
diskcopy [<drive1>: [<drive2>:]] [/v]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<drive1>` | Specifica l'unità che contiene il disco di origine. |
| /v | Verifica che le informazioni vengano copiate correttamente. Questa opzione rallenta il processo di copia. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- **Diskcopy** funziona solo con dischi rimovibili, ad esempio dischi floppy, che devono essere dello stesso tipo. Non è possibile usare **diskcopy** con un disco rigido. Se si specifica un'unità disco rigido per *unità1* o *Unità2*, in **diskcopy** viene visualizzato il messaggio di errore seguente:

    ```
    Invalid drive specification
    Specified drive does not exist or is nonremovable
    ```

    Il comando **diskcopy** richiede di inserire i dischi di origine e di destinazione e attende che venga premuto un tasto sulla tastiera prima di continuare.

    Dopo la copia del disco, **diskcopy** Visualizza il messaggio seguente:

    ```
    Copy another diskette (Y/N)?
    ```

    Se si preme **Y**, **diskcopy** richiede di inserire i dischi di origine e di destinazione per la successiva operazione di copia. Per arrestare il processo **diskcopy** , premere **N**.

    Se si esegue la copia in un disco floppy non formattato in *Unità2*, **diskcopy** formatta il disco con lo stesso numero di lati e settori per ogni traccia del disco in *unità1*. **Diskcopy** Visualizza il messaggio seguente mentre formatta il disco e copia i file:

    ```
    Formatting while copying
    ```

- Se il disco di origine ha un numero di serie del volume, **diskcopy** crea un nuovo numero di serie del volume per il disco di destinazione e visualizza il numero al termine dell'operazione di copia.

- Se si omette il parametro *Unità2* , **diskcopy** usa l'unità corrente come unità di destinazione. Se si omettono entrambi i parametri di unità, **diskcopy** usa l'unità corrente per entrambi. Se l'unità corrente è uguale a *unità1*, **diskcopy** richiede di scambiare dischi secondo necessità.

- Eseguire **diskcopy** da un'unità diversa dall'unità disco floppy, ad esempio l'unità C. Se *unità1* e *Unità2* del disco floppy sono uguali, **diskcopy** richiede di cambiare i dischi. Se i dischi contengono più informazioni rispetto alla memoria disponibile, **diskcopy** non è in grado di leggere tutte le informazioni in una sola volta. **Diskcopy** legge dal disco di origine, scrive nel disco di destinazione e richiede di inserire nuovamente il disco di origine. Questo processo continua fino a quando non viene copiato l'intero disco.

- La frammentazione è la presenza di piccole aree di spazio inutilizzato su disco tra i file esistenti su un disco. Un disco di origine frammentato può rallentare il processo di ricerca, lettura o scrittura di file.

    Poiché **diskcopy** esegue una copia esatta del disco di origine nel disco di destinazione, qualsiasi frammentazione sul disco di origine viene trasferita nel disco di destinazione. Per evitare di trasferire la frammentazione da un disco a un altro, utilizzare il [comando copy](copy.md) o il [comando xcopy](xcopy.md) per copiare il disco. Poiché **copia** e **xcopy** copiano i file in modo sequenziale, il nuovo disco non è frammentato.

    > [!NOTE]
    > Non è possibile utilizzare **xcopy** per copiare un disco di avvio.

- codici di uscita di **diskcopy** :

    | Codice di uscita | Descrizione |
    | --------- | ----------- |
    | 0 | Operazione di copia completata |
    | 1 | Si è verificato un errore di lettura/scrittura non irreversibile |
    | 3 | Si è verificato un errore hardware irreversibile |
    | 4 | Si è verificato un errore di inizializzazione |

    Per elaborare i codici di uscita restituiti da **verrà**, è possibile usare la variabile di ambiente *errorlevel* nella riga di comando **if** in un programma batch.

## <a name="examples"></a>Esempi

Per copiare il disco nell'unità B sul disco nell'unità A, digitare:

```
diskcopy b: a:
```

Per utilizzare l'unità disco floppy a per copiare un disco floppy in un altro, passare prima all'unità C e quindi digitare:

DISKCOPY a:

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando xcopy](xcopy.md)

- [Copy (comando)](copy.md)
