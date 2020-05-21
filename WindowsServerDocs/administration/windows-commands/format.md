---
title: format
description: Argomento di riferimento per il comando Format, che formatta un disco per accettare i file di Windows.
ms.prod: windows-server
manager: dongill
ms.author: jgerend
ms.technology: storage
ms.topic: article
ms.assetid: 51ec7423-9a01-4219-868a-25d69cdcc832
author: jasongerend
ms.date: 10/16/2017
ms.openlocfilehash: abe05c4bf8174fd76cedc29f2bce1c83587c3d8a
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436986"
---
# <a name="format"></a>Formato

> Si applica a: Windows 10, Windows Server 2016

Formatta un disco per accettare i file di Windows. Per formattare un disco rigido, è necessario essere membri del gruppo Administrators.

> [!NOTE]
> Dalla console di ripristino è inoltre possibile utilizzare il comando **Format** , con parametri diversi. Per ulteriori informazioni sulla console di ripristino di sistema, vedere [ambiente ripristino Windows (Windows re)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Sintassi

```
format <volume> [/fs:{FAT|FAT32|NTFS}] [/v:<label>] [/q] [/a:<unitsize>] [/c] [/x] [/p:<passes>]
format <volume> [/v:<label>] [/q] [/f:<size>] [/p:<passes>]
format <volume> [/v:<label>] [/q] [/t:<tracks> /n:<sectors>] [/p:<passes>]
format <volume> [/v:<label>] [/q] [/p:<passes>]
format <volume> [/q]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<volume>` | Specifica il punto di montaggio, il nome del volume o la lettera di unità (seguita da due punti) dell'unità che si desidera formattare. Se non si specifica alcuna delle opzioni della riga di comando seguenti, **Format** usa il tipo di volume per determinare il formato predefinito per il disco. |
| /FS: {FAT | FAT32 | NTFS | Specifica il tipo di file system (FAT, FAT32, NTFS). |
| /v:`<label>` | Specifica l'etichetta di volume. Se si omette l'opzione della riga di comando **/v** o la si utilizza senza specificare un'etichetta di volume, **Format** richiede l'etichetta del volume al termine della formattazione. Usare la sintassi **/v:** per non richiedere un'etichetta di volume. Se si usa un comando **format** singolo per formattare più di un disco, ai dischi verrà assegnata la stessa etichetta di volume. |
| /a`<unitsize>` | Specifica le dimensioni dell'unità di allocazione da utilizzare nei volumi FAT, FAT32 o NTFS. Se non si specifica *dimensioneunità*, viene scelto in base alle dimensioni del volume. Le impostazioni predefinite sono fortemente consigliate per l'uso generale. L'elenco seguente contiene i valori validi per NTFS, FAT e FAT32 *dimensioneunità*:<ul><li>512</li><li>1024</li><li>2048</li><li>4096</li><li>8192</li><li>16K</li><li>32 KB</li><li>64 KB</li></ul>FAT e FAT32 supportano anche 128 KB e 256 KB per una dimensione di settore maggiore di 512 byte. |
| /q | Esegue una formattazione veloce. Elimina la tabella file e la directory radice di un volume formattato in precedenza, ma non esegue un'analisi settoriale per settore per le aree non valide. È consigliabile usare l'opzione della riga di comando **/q** per formattare solo i volumi formattati in precedenza che sono in buone condizioni. Si noti che **/q** sostituisce **/p**. |
| /f`<size>` | Specifica la dimensione del disco floppy da formattare. Quando possibile, usare questa opzione della riga di comando anziché le opzioni **/t** e **/n** della riga di comando. Windows accetta i valori per le dimensioni seguenti:<ul><li>1440 o 1440k o 1440kb</li><li>1,44 o 1.44 m o 1.44 MB</li><li>1,44-MB, doppio lato, densità quadrupla, disco 3,5-pollice</li></ul> |
| /t:`<tracks>` | Specifica il numero di tracce del disco. Quando possibile, usare invece l'opzione della riga di comando **/f** . Se si usa l'opzione **/t**, è necessario usare anche l'opzione **/n**. Queste opzioni offrono insieme un metodo alternativo per specificare la dimensione del disco che viene formattato. Questa opzione non è valida con l'opzione **/f**. |
| /n`<sectors>` | Specifica il numero di settori per traccia. Quando possibile, usare l'opzione della riga di comando **/f** anziché **/n**. Se si usa **/n**, è necessario usare anche **/t**. Queste due opzioni offrono insieme un metodo alternativo per specificare la dimensione del disco che viene formattato. Questa opzione non è valida con l'opzione **/f**. |
| /p`<passes>` | Azzera ogni settore nel volume per il numero di passaggi specificato. Questa opzione non è valida con l'opzione **/q**. |
| /C | Solo NTFS. I file creati sul nuovo volume verranno compressi per impostazione predefinita. |
| /x | Causa lo smontaggio del volume, se necessario, prima che venga formattato. Tutti gli handle aperti per il volume non saranno più validi. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Il comando **Format** crea una nuova directory radice e file System per il disco. Può inoltre verificare la presenza di aree non valide sul disco e può eliminare tutti i dati sul disco. Per poter utilizzare un nuovo disco, è innanzitutto necessario utilizzare questo comando per formattare il disco.

- Dopo la formattazione di un disco floppy, **Format** Visualizza il messaggio seguente:

    `Volume label (11 characters, ENTER for none)?`

    Per aggiungere un'etichetta di volume, digitare fino a 11 caratteri (inclusi gli spazi). Se non si desidera aggiungere un'etichetta di volume al disco, premere INVIO.

- Quando si usa il comando **Format** per formattare un disco rigido, viene visualizzato un messaggio di avviso simile al seguente:

  ```
  WARNING, ALL DATA ON NON-REMOVABLE DISK
  DRIVE x: WILL BE LOST!
  Proceed with Format (Y/N)? _
  ```

  Per formattare il disco rigido, premere **Y**; Se non si desidera formattare il disco, premere **N**.

- I file system FAT limitano il numero di cluster a un massimo di 65526. I file system FAT32 limitano il numero di cluster a tra 65527 e 4177917.

- La compressione NTFS non è supportata per le dimensioni di unità di allocazione superiori a 4096.

  > [!NOTE]
  > Il **formato** arresterà immediatamente l'elaborazione se determina che i requisiti precedenti non possono essere soddisfatti con le dimensioni del cluster specificate.

- Al termine della formattazione, **Format** Visualizza i messaggi che mostrano lo spazio totale su disco, gli spazi contrassegnati come difettosi e lo spazio disponibile per i file.

- È possibile velocizzare il processo di formattazione utilizzando l'opzione della riga di comando **/q** . Usare questa opzione solo se non sono presenti settori danneggiati sul disco rigido.

- Non è consigliabile usare il comando **Format** in un'unità preparata usando il comando **SUBST** . Non è possibile formattare i dischi in una rete.

- Nella tabella seguente sono elencati i codici di uscita con una breve descrizione del loro significato.

  | Codice di uscita | Descrizione |
  | --------- | ----------- |
  | 0 | L'operazione di formattazione è stata completata. |
  | 1 | Sono stati specificati parametri non corretti. |
  | 4 | Si è verificato un errore irreversibile, ovvero un errore diverso da 0, 1 o 5. |
  | 5 | L'utente ha premuto N in risposta alla richiesta "procedere con il formato (Y/N)?" per arrestare il processo. |

  È possibile controllare i codici di uscita usando la variabile di ambiente ERRORLEVEL con il comando batch **if**.

### <a name="examples"></a>Esempi

Per formattare un nuovo disco floppy nell'unità A usando le dimensioni predefinite, digitare:

```
format a:
```

Per eseguire un'operazione di formattazione veloce su un disco floppy formattato in precedenza nell'unità A, digitare:

```
format a: /q
```

Per formattare un disco floppy nell'unità A e assegnargli i *dati*dell'etichetta del volume, digitare:

```
format a: /v:DATA
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](https://technet.microsoft.com/library/cc771080.aspx)
