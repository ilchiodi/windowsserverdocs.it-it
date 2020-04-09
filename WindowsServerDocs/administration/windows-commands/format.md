---
title: Formato
ms.prod: windows-server
manager: dongill
ms.author: jgerend
ms.technology: storage
ms.topic: article
ms.assetid: 51ec7423-9a01-4219-868a-25d69cdcc832
author: jasongerend
ms.date: 10/16/2017
ms.openlocfilehash: 95f88ef316bb7d188db212911835b867c6d4556f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844474"
---
# <a name="format"></a>Formato
> Si applica a: Windows 10, Windows Server 2016

Formatta un disco per accettare i file di Windows.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
format <Volume> [/fs:{FAT|FAT32|NTFS}] [/v:<Label>] [/q] [/a:<UnitSize>] [/c] [/x] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/f:<Size>] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/t:<Tracks> /n:<Sectors>] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/p:<Passes>]
format <Volume> [/q]
```

### <a name="parameters"></a>Parametri

|   Parametro    |                                                                                                                                                                                                                    Descrizione                                                                                                                                                                                                                     |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   \<volume >    |                                                                                         Specifica il punto di montaggio, il nome del volume o la lettera di unità (seguita da due punti) dell'unità che si desidera formattare. Se non si specifica alcuna delle opzioni della riga di comando seguenti, **Format** usa il tipo di volume per determinare il formato predefinito per il disco.                                                                                         |
|    /FS: {FAT    |                                                                                                                                                                                                                       FAT32                                                                                                                                                                                                                        |
|  /v: etichetta\<>   |                           Specifica l'etichetta di volume. Se si omette l'opzione della riga di comando **/v** o la si utilizza senza specificare un'etichetta di volume, **Format** richiede l'etichetta del volume al termine della formattazione. Usare la sintassi **/v:** per non richiedere un'etichetta di volume. Se si usa un comando **format** singolo per formattare più di un disco, ai dischi verrà assegnata la stessa etichetta di volume.                            |
| /a:\<dimensioneunità > | Specifica le dimensioni dell'unità di allocazione da utilizzare nei volumi FAT, FAT32 o NTFS. Se non si specifica *UnitSize*, la dimensione viene scelta in base alle dimensioni del volume. Le impostazioni predefinite sono fortemente consigliate per l'uso generale. Nell'elenco seguente sono riportati i valori validi di *UnitSize* per NTFS, FAT e FAT32:</br>512</br>1024</br>2048</br>4096</br>8192</br>16 KB</br>32 KB</br>64 KB</br>FAT e FAT32 supportano anche 128 KB e 256 KB per una dimensione di settore maggiore di 512 byte. |
|       /q       |                                                       Esegue una formattazione veloce. Elimina la tabella file e la directory radice di un volume formattato in precedenza, ma non esegue un'analisi settoriale per settore per le aree non valide. È consigliabile usare l'opzione della riga di comando **/q** per formattare solo i volumi formattati in precedenza che sono in buone condizioni. Si noti che **/q** sostituisce **/p**.                                                       |
|   /f: dimensioni\<>   |                                                         Specifica la dimensione del disco floppy da formattare. Quando possibile, usare questa opzione della riga di comando anziché le opzioni **/t** e **/n** della riga di comando. Windows accetta i valori per le dimensioni seguenti:</br>-   1440 o 1440 K o 1440 KB</br>-   1,44 o 1,44 M o 1,44 MB</br>-1,44-MB, doppio lato, densità quadrupla, disco da 3,5 pollici                                                         |
|  /t:\<tiene traccia >  |                                                    Specifica il numero di tracce del disco. Quando possibile, usare invece l'opzione della riga di comando **/f** . Se si usa l'opzione **/t**, è necessario usare anche l'opzione **/n**. Queste opzioni offrono insieme un metodo alternativo per specificare la dimensione del disco che viene formattato. Questa opzione non è valida con l'opzione **/f**.                                                     |
| /n:\<settori >  |                                                         Specifica il numero di settori per traccia. Quando possibile, usare l'opzione della riga di comando **/f** anziché **/n**. Se si usa **/n**, è necessario usare anche **/t**. Queste due opzioni offrono insieme un metodo alternativo per specificare la dimensione del disco che viene formattato. Questa opzione non è valida con l'opzione **/f**.                                                         |
|  /p:\<passa >  |                                                                                                                                                               Azzera ogni settore nel volume per il numero di passaggi specificato. Questa opzione non è valida con l'opzione **/q**.                                                                                                                                                                |
|       /c       |                                                                                                                                                                                     Solo NTFS. I file creati sul nuovo volume verranno compressi per impostazione predefinita.                                                                                                                                                                                      |
|       /x       |                                                                                                                                                            Causa lo smontaggio del volume, se necessario, prima della formattazione. Tutti gli handle aperti per il volume non saranno più validi.                                                                                                                                                            |
|       /?       |                                                                                                                                                                                                        Visualizza la guida al prompt dei comandi.                                                                                                                                                                                                        |

## <a name="remarks"></a>Note

-   Credenziali amministrative

    Per formattare un disco rigido, è necessario essere membri del gruppo Administrators.
-   Uso del **formato**

    Il comando **Format** crea una nuova directory radice e file System per il disco. Può inoltre verificare la presenza di aree non valide sul disco e può eliminare tutti i dati sul disco. Per poter utilizzare un nuovo disco, è innanzitutto necessario utilizzare questo comando per formattare il disco.
-   Aggiunta di un'etichetta di volume

    Dopo la formattazione di un disco floppy, **Format** Visualizza il messaggio seguente:

    `Volume label (11 characters, ENTER for none)?`

    Per aggiungere un'etichetta di volume, digitare fino a 11 caratteri (inclusi gli spazi). Se non si desidera aggiungere un'etichetta di volume al disco, premere INVIO.
-   Formattazione di un disco rigido

> [!NOTE]
> Per formattare un disco rigido, è necessario essere membri del gruppo Administrators.

Quando si usa il comando **Format** per formattare un disco rigido, viene visualizzato un messaggio di avviso simile al seguente:
```
WARNING, ALL DATA ON NON-REMOVABLE DISK 
DRIVE x: WILL BE LOST! 
Proceed with Format (Y/N)? _ 
```
Per formattare il disco rigido, premere Y; Se non si desidera formattare il disco, premere N.
-   Dimensioni unità

    I file system FAT limitano il numero di cluster a un massimo di 65526. I file system FAT32 limitano il numero di cluster a tra 65527 e 4177917.

    La compressione NTFS non è supportata per le dimensioni di unità di allocazione superiori a 4096.

> [!NOTE]
> Il **formato** arresterà immediatamente l'elaborazione se determina che i requisiti precedenti non possono essere soddisfatti con le dimensioni del cluster specificate.
> -   **Formattare** i messaggi

    When formatting is complete, **format** displays messages that show the total disk space, the spaces marked as defective, and the space available for your files.
- Formattazione veloce

  È possibile velocizzare il processo di formattazione utilizzando l'opzione della riga di comando **/q** . Usare questa opzione solo se non sono presenti settori danneggiati sul disco rigido.
- Uso di **format** con un'unità o un'unità di rete riassegnata

  Non è consigliabile usare il comando **format** in un'unità preparata usando il comando **subst**. Non è possibile formattare i dischi in una rete.
- Codici di uscita di **Format**

  Nella tabella seguente sono elencati i codici di uscita con una breve descrizione del loro significato.  

  |Codice di uscita|Descrizione|
  |---------|-----------|
  |0|L'operazione di formattazione è stata completata.|
  |1|Sono stati specificati parametri non corretti.|
  |4|Si è verificato un errore irreversibile, ovvero un errore diverso da 0, 1 o 5.|
  |5|L'utente ha premuto N in risposta alla richiesta "procedere con il formato (Y/N)?" per arrestare il processo.|

  È possibile controllare i codici di uscita usando la variabile di ambiente ERRORLEVEL con il comando batch **if**.
- Uso di **format** nella Console di ripristino

  Il comando **format** con parametri diversi è disponibile dalla Console di ripristino.

## <a name="examples"></a><a name="BKMK_examples"></a>Esempi

Per formattare un nuovo disco floppy nell'unità A usando le dimensioni predefinite, digitare:
```
format a:
```
Per eseguire un'operazione di formattazione veloce su un disco floppy formattato in precedenza nell'unità A, digitare:
```
format a: /q
```
Per formattare un disco floppy nell'unità A e assegnare l'etichetta di volume "DATA", digitare:
```
format a: /v:DATA
```

## <a name="additional-references"></a>Altre informazioni di riferimento

[Indicazioni generali sulla sintassi della riga di comando](https://technet.microsoft.com/library/cc771080.aspx)