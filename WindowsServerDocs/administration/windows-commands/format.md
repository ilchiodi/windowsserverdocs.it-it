---
title: Formato
ms.prod: windows-server-threshold
manager: dongill
ms.author: JGerend
ms.technology: storage
ms.topic: article
ms.assetid: 51ec7423-9a01-4219-868a-25d69cdcc832
author: JasonGerend
ms.date: 10/16/2017
ms.openlocfilehash: 1af340f4612015ede3fee659c9662955df9aa578
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439271"
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

## <a name="parameters"></a>Parametri

|   Parametro    |                                                                                                                                                                                                                    Descrizione                                                                                                                                                                                                                     |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   \<Volume >    |                                                                                         Specifica il punto di montaggio, il nome del volume o lettera di unità (seguita da due punti) dell'unità che si desidera formattare. Se non si specifica una delle opzioni della riga di comando seguente, **formato** utilizza il tipo di volume per determinare il formato predefinito per il disco.                                                                                         |
|    /FS: {FAT    |                                                                                                                                                                                                                       FAT32                                                                                                                                                                                                                        |
|  /v:\<Label>   |                           Specifica l'etichetta di volume. Se si omette il **/v** della riga di comando opzione o usarlo senza specificare un'etichetta volume **formato** richiede l'etichetta di volume dopo la formattazione è stata completata. Usare la sintassi **/v:** per non richiedere un'etichetta di volume. Se si usa un comando **format** singolo per formattare più di un disco, ai dischi verrà assegnata la stessa etichetta di volume.                            |
| /a:\<UnitSize> | Specifica la dimensione di unità di allocazione da usare nei volumi FAT, FAT32 o NTFS. Se non si specifica *UnitSize*, la dimensione viene scelta in base alle dimensioni del volume. Le impostazioni predefinite sono fortemente consigliate per l'uso generale. Nell'elenco seguente sono riportati i valori validi di *UnitSize* per NTFS, FAT e FAT32:</br>512</br>1024</br>2048</br>4096</br>8192</br>16 KB</br>32 KB</br>64 KB</br>FAT e FAT32 supportano anche 128 KB e 256 KB per una dimensione di settore maggiore di 512 byte. |
|       /q       |                                                       Esegue una formattazione veloce. Elimina la tabella di file e directory radice di un volume formattato in precedenza, ma non esegue un'analisi settore per settore per le aree danneggiate. È consigliabile usare la **/q** opzione della riga di comando per formattare solo formattato in precedenza i volumi che si è certi siano in buono stato. Si noti che **/q** sostituisce **/p**.                                                       |
|   /f:\<Size>   |                                                         Specifica la dimensione del disco floppy da formattare. Quando possibile, usare questa opzione della riga di comando anziché la **/t** e **/n** opzioni della riga di comando. Windows accetta i valori per le dimensioni seguenti:</br>-   1440 o 1440 K o 1440 KB</br>-   1,44 o 1,44 M o 1,44 MB</br>-Disco da 1,44 MB, fronte-retro, densità quadrupla, da 3,5 pollici                                                         |
|  /t:\<tracce >  |                                                    Specifica il numero di tracce del disco. Quando possibile, usare il **/f** della riga di comando invece l'opzione. Se si usa l'opzione **/t**, è necessario usare anche l'opzione **/n**. Queste opzioni offrono insieme un metodo alternativo per specificare la dimensione del disco che viene formattato. Questa opzione non è valida con l'opzione **/f**.                                                     |
| /n:\<Sectors>  |                                                         Specifica il numero di settori per traccia. Quando possibile, usare il **/f** opzione della riga di comando anziché **/n**. Se si usa **/n**, è necessario usare anche **/t**. Queste due opzioni offrono insieme un metodo alternativo per specificare la dimensione del disco che viene formattato. Questa opzione non è valida con l'opzione **/f**.                                                         |
|  /p:\<Passes>  |                                                                                                                                                               Azzera ogni settore nel volume per il numero di passaggi specificato. Questa opzione non è valida con l'opzione **/q**.                                                                                                                                                                |
|       /c       |                                                                                                                                                                                     Solo NTFS. I file creati sul nuovo volume verranno compressi per impostazione predefinita.                                                                                                                                                                                      |
|       /x       |                                                                                                                                                            Causa lo smontaggio del volume, se necessario, prima della formattazione. Tutti gli handle aperti per il volume non saranno più validi.                                                                                                                                                            |
|       /?       |                                                                                                                                                                                                        Visualizza la guida al prompt dei comandi.                                                                                                                                                                                                        |

## <a name="remarks"></a>Note

-   Credenziali amministrative

    È necessario essere un membro del gruppo Administrators per formattare un disco rigido.
-   Usando **formato**

    Il **formato** comando crea un nuovo sistema file e directory di radice di per il disco. Anche possibile cercare le aree danneggiate nel disco e possibile eliminare tutti i dati sul disco. Per poter usare un nuovo disco, è innanzitutto necessario utilizzare questo comando per formattare il disco.
-   Aggiunta di un'etichetta di volume

    Dopo la formattazione di un disco floppy **formato** viene visualizzato il messaggio seguente:

    `Volume label (11 characters, ENTER for none)?`

    Per aggiungere un'etichetta di volume, digitare fino a 11 caratteri (spazi inclusi). Se si desidera aggiungere un'etichetta di volume sul disco, premere INVIO.
-   Formattazione di un disco rigido

> [!NOTE]
> È necessario essere un membro del gruppo Administrators per formattare un disco rigido.

Quando si usa la **formato** comando per formattare un disco rigido, un messaggio di avviso simile a verrà visualizzato il seguente:
```
WARNING, ALL DATA ON NON-REMOVABLE DISK 
DRIVE x: WILL BE LOST! 
Proceed with Format (Y/N)? _ 
```
Per formattare il disco rigido, premere Y; Se non si desidera formattare il disco, premere N.
-   Dimensioni dell'unità

    File system FAT limitano il numero di cluster a non più di 65526. File system FAT32 limitare il numero di cluster da un 65527 4177917.

    La compressione NTFS non è supportata per le dimensioni di unità di allocazione superiori a 4096.

> [!NOTE]
> **Formato** verrà interrotto immediatamente l'elaborazione se determina che i requisiti precedenti non possono essere soddisfatti utilizzando le dimensioni del cluster specificato.
> -   **Formato** messaggi

    When formatting is complete, **format** displays messages that show the total disk space, the spaces marked as defective, and the space available for your files.
- Formattazione veloce

  È possibile velocizzare il processo di formattazione usando il **/q** opzione della riga di comando. Usare questa opzione solo se non sono presenti settori danneggiati sul disco rigido.
- Uso di **format** con un'unità o un'unità di rete riassegnata

  Non è consigliabile usare il comando **format** in un'unità preparata usando il comando **subst**. Non è possibile formattare i dischi in una rete.
- Codici di uscita di **Format**

  Nella tabella seguente sono elencati i codici di uscita con una breve descrizione del loro significato.  

  |Codice di uscita|Descrizione|
  |---------|-----------|
  |0|L'operazione di formattazione è stata completata.|
  |1|Sono stati specificati parametri non corretti.|
  |4|Si è verificato un errore irreversibile (ovvero qualsiasi errore diverso da 0, 1 o 5).|
  |5|L'utente ha premuto N in risposta alla richiesta "Procedere con la formattazione (Y/N)?" per arrestare il processo.|

  È possibile controllare i codici di uscita usando la variabile di ambiente ERRORLEVEL con il comando batch **if**.
- Uso di **format** nella Console di ripristino

  Il comando **format** con parametri diversi è disponibile dalla Console di ripristino.

## <a name="BKMK_examples"></a>Esempi

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

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](https://technet.microsoft.com/library/cc771080.aspx)