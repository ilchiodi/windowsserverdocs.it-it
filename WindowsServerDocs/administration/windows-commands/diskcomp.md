---
title: diskcomp
description: Argomento di riferimento per il comando verrà, che consente di confrontare il contenuto di due dischi floppy.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4f56f534-a356-4daa-8b4f-38e089341e42
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fcb810f4cd18d51f8151b27a6f447c86130624fd
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992516"
---
# <a name="diskcomp"></a>diskcomp

Confronta il contenuto di due dischi floppy. Se usato senza parametri, **verrà** usa l'unità corrente per confrontare entrambi i dischi.

## <a name="syntax"></a>Sintassi

```
diskcomp [<drive1>: [<drive2>:]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<drive1>` | Specifica l'unità contenente uno dei dischi floppy. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Il comando **verrà** funziona solo con dischi floppy. Non è possibile usare **verrà** con un disco rigido. Se si specifica un'unità disco rigido per *unità1* o *Unità2*, in **verrà** viene visualizzato il messaggio di errore seguente:

  ```
  Invalid drive specification
  Specified drive does not exist
  or is nonremovable
  ```

- Se tutte le tracce sui due dischi confrontati sono uguali (ignora il numero di volume di un disco), **verrà** Visualizza il messaggio seguente:

  ```
  Compare OK
  ```

  Se le tracce non sono uguali, **verrà** Visualizza un messaggio simile al seguente:

  ```
  Compare error on
  side 1, track 2
  ```

  Quando **verrà** completa il confronto, viene visualizzato il messaggio seguente:

  ```
  Compare another diskette (Y/N)?
  ```

  Se si preme **Y**, **verrà** richiede di inserire il disco per il confronto successivo. Se si preme **N**, **verrà** interrompe il confronto.

- Se si omette il parametro *Unità2* , **verrà** usa l'unità corrente per *Unità2*. Se si omettono entrambi i parametri di unità, **verrà** usa l'unità corrente per entrambi. Se l'unità corrente è uguale a *unità1*, **verrà** richiede di scambiare dischi secondo necessità.

- Se si specifica la stessa unità disco floppy per *unità1* e *Unità2*, **verrà** le confronta usando un'unità e richiede di inserire i dischi in base alle esigenze. Potrebbe essere necessario scambiare i dischi più di una volta, a seconda della capacità dei dischi e della quantità di memoria disponibile.

- **Verrà** non è in grado di confrontare un disco su un solo lato con un disco a doppio lato, né un disco ad alta densità con un disco a densità doppia. Se il disco in *unità1* non è dello stesso tipo del disco in *Unità2*, **verrà** Visualizza il messaggio seguente:

  ```
  Drive types or diskette types not compatible
  ```

- **Verrà** non funziona in un'unità di rete o in un'unità creata dal comando **SUBST** . Se si tenta di utilizzare **verrà** con un'unità di uno di questi tipi, **verrà** Visualizza il messaggio di errore seguente:

  ```
  Invalid drive specification
  ```

- Se si usa **verrà** con un disco creato usando **Copy**, **verrà** potrebbe visualizzare un messaggio simile al seguente:

  ```
  Compare error on
  side 0, track 0
  ```

  Questo tipo di errore può verificarsi anche se i file sui dischi sono identici. Sebbene **copia** le informazioni duplicate, non lo inserisce necessariamente nello stesso percorso nel disco di destinazione.

- codici di uscita di **verrà** :

  | Codice di uscita | Descrizione |
  | --------- | ----------- |
  | 0 | I dischi sono uguali |
  | 1 | Sono state trovate differenze |
  | 3 | Si è verificato un errore hardware |
  | 4 | Si è verificato un errore di inizializzazione |

  Per elaborare i codici di uscita restituiti da **verrà**, è possibile usare la variabile di ambiente *errorlevel* nella riga di comando **if** in un programma batch.

## <a name="examples"></a>Esempi

Se il computer dispone di una sola unità disco floppy, ad esempio l'unità A, e si desidera confrontare due dischi, digitare:

```
diskcomp a: a:
```

**Verrà** richiede di inserire ogni disco, in base alle esigenze.

Per illustrare come elaborare un codice di uscita di **verrà** in un programma batch che usa la variabile di ambiente *errorlevel* nella riga di comando **if** :

```
rem Checkout.bat compares the disks in drive A and B
echo off
diskcomp a: b:
if errorlevel 4 goto ini_error
if errorlevel 3 goto hard_error
if errorlevel 1 goto no_compare
if errorlevel 0 goto compare_ok
:ini_error
echo ERROR: Insufficient memory or command invalid
goto exit
:hard_error
echo ERROR: An irrecoverable error occurred
goto exit
:break
echo You just pressed CTRL+C to stop the comparison
goto exit
:no_compare
echo Disks are not the same
goto exit
:compare_ok
echo The comparison was successful; the disks are the same
goto exit
:exit
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
