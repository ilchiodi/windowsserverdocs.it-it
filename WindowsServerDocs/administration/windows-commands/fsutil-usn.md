---
ms.assetid: faad34aa-4ba1-4129-bc1f-08088399e2fa
title: Fsutil usn
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 4bef63f4938b5ce786bbbfdbf3bdd2081280ce95
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829982"
---
# <a name="fsutil-usn"></a>Fsutil usn
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gestisce il journal delle modifiche USN (number) sequenza update.

## <a name="syntax"></a>Sintassi

```
fsutil usn [createjournal] m=<MaxSize> a=<AllocationDelta> <VolumePath>
fsutil usn [deletejournal] {/D | /N} <volumepath>
fsutil usn [enablerangetracking] <volumepath> [options]
fsutil usn [enumdata] <FileRef> <LowUSN> <HighUSN> <VolumePath>
fsutil usn [queryjournal] <VolumePath>
fsutil usn [readdata] <FileName>
fsutil usn [readjournal] [c= <chunk-size> s=<file-size-threshold>] <volumepath>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|createjournal|Crea un journal delle modifiche USN.|
|m =\<MaxSize >|Specifica la dimensione massima, espressa in byte, che consente di allocare NTFS per il journal delle modifiche.|
|un =\<DeltaAllocazione >|Specifica la dimensione, espressa in byte, dell'allocazione di memoria che verrà aggiunto alla fine e rimosso dall'inizio nel journal delle modifiche.|
|\<VolumePath>|Specifica la lettera di unità (seguita da due punti).|
|deletejournal|Elimina o disattiva un journal delle modifiche USN attivo. **Attenzione:** Eliminare il journal delle modifiche interessa il servizio Replica File (FRS) e il servizio di indicizzazione, in quanto richiederebbe questi servizi per eseguire un'analisi completa (e che richiedono molto tempo) del volume. Questo influisce a sua volta negativamente sulle replica FRS SYSVOL e replica tra gli oggetti alternativi collegamento DFS mentre il volume viene viene ripetuta l'analisi.|
|/d|Disabilita un journal delle modifiche USN active e restituisce il controllo di input/output (i/o) mentre il journal delle modifiche è stato disabilitato.|
|/n|Disabilita un journal delle modifiche USN active e restituisce il controllo dei / o solo dopo che il journal delle modifiche è disabilitato.|
|enablerangetracking|Consente l'intervallo di scrittura USN di rilevamento per un volume.|
|c=\<chunk-size>|Specifica le dimensioni del blocco per tenere traccia in un volume.|
|s=\<file-size-threshold>|Specifica la soglia delle dimensioni di file per l'intervallo di rilevamento.|
|enumdata|Enumera e sono elencate le voci del journal delle modifiche tra due limiti specificati.|
|\<FileRef>|Specifica la posizione ordinale all'interno dei file nel volume di cui l'enumerazione ha inizio.|
|\<UsnInferiore >|Specifica il limite inferiore dell'intervallo di valori USN utilizzato per filtrare i record restituiti. Solo record il cui ultimo journal USN è uguale a o compreso tra il *UsnInferiore* e *HighUSN* vengono restituiti i valori del membro.|
|\<HighUSN >|Specifica il limite superiore dell'intervallo di valori USN utilizzato per filtrare i file che vengono restituiti.|
|queryjournal restituisce|Esegue query sui dati di USN di un volume per raccogliere informazioni sul journal delle modifiche corrente, i record e alla capacità.|
|readdata|Legge i dati di USN di un file.|
|\<FileName>|Specifica il percorso completo del file, incluso il nome del file e l'estensione, ad esempio: C:\documents\filename.txt|
|readjournal|Legge i record degli USN nel journal USN.|
|MinVer =\<numero >|Versione principale minima del USN_RECORD da restituire. Default = 2.|
|MaxVer =\<numero >|Numero di versione principale di numero massimo di USN_RECORD da restituire. Predefinito = 4.|
|startusn =\<numero USN >|USN iniziare la lettura nel journal USN da. Predefinito = 0.|


## <a name="remarks"></a>Note

-   Sul journal delle modifiche USN

    Il journal delle modifiche USN fornisce un registro permanente di tutte le modifiche apportate ai file nel volume. Come file, directory e altri oggetti NTFS vengono aggiunti, eliminati e modificati, NTFS immette i record il journal delle modifiche USN, uno per ogni volume del computer. In ogni record sono indicati il tipo di modifica e l'oggetto modificato. Nuovi record vengono aggiunti alla fine del flusso.

    I programmi possono consultare il journal delle modifiche USN per determinare tutte le modifiche apportate a un set di file. Il journal delle modifiche USN è più efficace rispetto al controllo timbri data / ora o la registrazione per le notifiche file. Il journal delle modifiche USN è abilitato e usato dal servizio di indicizzazione, servizio Replica File (FRS), servizi di installazione remota (RIS) e l'archiviazione remota.

-   Usando **createjournal**

    Se esiste già un journal delle modifiche in un volume, il **createjournal** parametro aggiorna il journal delle modifiche *MaxSize* e *DeltaAllocazione* parametri. In questo modo è possibile aumentare il numero di record che conserva un giornale di registrazione active senza la necessità di disabilitarlo.

-   Usando **m**

    Il journal delle modifiche può raggiungere dimensioni superiori a questo valore di destinazione, ma il journal delle modifiche viene troncato in corrispondenza del checkpoint successivo NTFS su un valore inferiore rispetto a questo valore. Esamina il journal delle modifiche e lo rimuove quando la relativa dimensione supera il valore di NTFS *MaxSize* sommato al valore di *DeltaAllocazione*. In Checkpoint NTFS, il sistema operativo scrive i record per il file di log NTFS che consentono di NTFS determinare il tipo di elaborazione è necessario per recuperare da un errore.

-   Usando **un**

    Il journal delle modifiche possono aumentare fino a più della somma dei valori di *MaxSize* e *DeltaAllocazione* prima da cui vengono eliminati.

-   Usando **deletejournal**

    Eliminazione o disabilitazione di un journal delle modifiche attive è richiedere molto tempo, perché il sistema deve accedere a tutti i record nella tabella file master (MFT) e impostare l'attributo USN ultimo su 0 (zero). Questo processo può richiedere alcuni minuti e può continuare dopo il riavvio del sistema, se è necessario un riavvio. Durante questo processo, il journal delle modifiche viene considerato non attivo, né è disabilitata. Mentre il sistema di disabilitazione del diario, non è accessibile e tutte le operazioni di registrazione restituiscono errori. È necessario utilizzare prestare particolare attenzione quando si disabilita un giornale di registrazione attivo, in quanto influisce negativamente su altre applicazioni che usano il journal.

## <a name="BKMK_examples"></a>Esempi
Per creare un USN journal delle modifiche sull'unità C, digitare:

```
fsutil usn createjournal m=1000 a=100 c:
```

Per eliminare un oggetto attivo USN journal delle modifiche sull'unità C, digitare:

```
fsutil usn deletejournal /d c:
```

Per abilitare l'intervallo di rilevamento con una dimensione del blocco specificata e la soglia di dimensioni del file, digitare:

```
fsutil usn enablerangetracking c=16384 s=67108864 C:
```

Per enumerare e visualizzare le voci di journal delle modifiche tra due limiti specificati nell'unità C, digitare:

```
fsutil usn enumdata 1 0 1 c:
```

Per eseguire query sui dati di USN per un volume sull'unità C, digitare:

```
fsutil usn queryjournal c:
```

Per leggere i dati USN di un file nella cartella \Temp sull'unità C, digitare:

```
fsutil usn readdata c:\temp\sample.txt
```

Per leggere il journal USN con un USN specifico start, digitare:

```
fsutil usn readjournal startusn=0xF00
```

#### <a name="additional-references"></a>Altri riferimenti
[Chiave sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


