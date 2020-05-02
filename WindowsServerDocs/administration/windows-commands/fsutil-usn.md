---
ms.assetid: faad34aa-4ba1-4129-bc1f-08088399e2fa
title: Fsutil usn
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: d17246b03de20d0bc327af801621a28f406edf63
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720076"
---
# <a name="fsutil-usn"></a>Fsutil usn
> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gestisce il Journal delle modifiche del numero di sequenza di aggiornamento (USN).

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

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|createjournal|Crea un journal delle modifiche USN.|
|m =\<MaxSize>|Specifica la dimensione massima, in byte, allocata da NTFS per il Journal delle modifiche.|
|a =\<DeltaAllocazione>|Specifica la dimensione, in byte, dell'allocazione di memoria che viene aggiunta alla fine e rimossa dall'inizio del journal delle modifiche.|
|\<> VolumePath|Specifica la lettera di unità (seguita da due punti).|
|deletejournal|Elimina o Disabilita un journal delle modifiche Active USN. **Attenzione:** L'eliminazione del journal delle modifiche influisca sul servizio Replica file (FRS) e sul servizio di indicizzazione, perché richiederebbe a questi servizi di eseguire un'analisi completa e dispendiosa in termini di tempo del volume. Questo, a sua volta, influisce negativamente sulla replica SYSVOL di FRS e sulla replica tra le alternative dei collegamenti DFS mentre è in corso la scansione del volume.|
|/d|Disabilita un journal delle modifiche Active USN e restituisce il controllo di input/output (I/O) mentre è in corso la disabilitazione del journal delle modifiche.|
|/n|Disabilita un journal delle modifiche Active USN e restituisce il controllo I/O solo dopo la disabilitazione del journal delle modifiche.|
|enablerangetracking|Abilita il rilevamento dell'intervallo di scrittura USN per un volume.|
|c =\<dimensioni del blocco>|Specifica la dimensione del blocco di cui tenere traccia in un volume.|
|s =\<dimensione file-soglia>|Specifica la soglia delle dimensioni del file per il rilevamento dell'intervallo.|
|EnumData|Enumera ed elenca le voci del journal delle modifiche tra due limiti specificati.|
|\<> FileRef|Specifica la posizione ordinale all'interno dei file nel volume da cui deve iniziare l'enumerazione.|
|\<> LowUSN|Specifica il limite inferiore dell'intervallo di valori USN usato per filtrare i record restituiti. Vengono restituiti solo i record il cui ultimo USN del journal delle modifiche è compreso tra o uguale ai valori del membro *LowUSN* e *HighUSN* .|
|\<> HighUSN|Specifica il limite superiore dell'intervallo di valori USN utilizzati per filtrare i file restituiti.|
|queryjournal|Esegue una query sui dati USN di un volume per raccogliere informazioni sul Journal delle modifiche corrente, i relativi record e la relativa capacità.|
|ReadData|Legge i dati USN per un file.|
|\<> FileName|Specifica il percorso completo del file, inclusi il nome file e l'estensione, ad esempio: C:\documents\filename.txt|
|aggiornamento|Legge i record USN nel journal USN.|
|Minver =\<numero>|Versione principale minima di USN_RECORD da restituire. Valore predefinito = 2.|
|MaxVer =\<numero>|Versione principale massima di USN_RECORD da restituire. Valore predefinito = 4.|
|startusn =\<numero USN>|USN per iniziare a leggere il journal USN da. Valore predefinito = 0.|


## <a name="remarks"></a>Osservazioni

-   Informazioni sul Journal delle modifiche USN

    Il Journal delle modifiche USN fornisce un log permanente di tutte le modifiche apportate ai file nel volume. Quando vengono aggiunti, eliminati e modificati file, directory e altri oggetti NTFS, NTFS immette i record nel journal delle modifiche USN, uno per ogni volume del computer. In ogni record sono indicati il tipo di modifica e l'oggetto modificato. I nuovi record vengono aggiunti alla fine del flusso.

    I programmi possono consultare il Journal delle modifiche USN per determinare tutte le modifiche apportate a un set di file. Il Journal delle modifiche USN è molto più efficiente rispetto alla verifica dei timestamp o alla registrazione per le notifiche di file. Il Journal delle modifiche USN è abilitato e utilizzato dal servizio di indicizzazione, dal servizio Replica file (FRS), dai servizi di installazione remota (RIS) e dall'archiviazione remota.

-   Uso di **createjournal**

    Se in un volume è già presente un journal delle modifiche, il parametro **createjournal** aggiorna i parametri *MaxSize* e *DeltaAllocazione* del journal delle modifiche. In questo modo è possibile espandere il numero di record gestiti da un journal attivo senza che sia necessario disabilitarlo.

-   Uso di **m**

    Il Journal delle modifiche può aumentare di dimensioni superiori rispetto a questo valore di destinazione, ma il Journal delle modifiche viene troncato al successivo checkpoint NTFS a un valore inferiore a quello specificato. NTFS esamina il Journal delle modifiche e lo taglia quando la relativa dimensione supera il valore di *MaxSize* più il valore di *DeltaAllocazione*. Ai checkpoint NTFS, il sistema operativo scrive i record nel file di log NTFS che consentono a NTFS di determinare quale elaborazione è necessaria per il ripristino da un errore.

-   Uso **di un**

    Il Journal delle modifiche può aumentare fino a raggiungere la somma dei valori di *MaxSize* e *DeltaAllocazione* prima di essere eliminati.

-   Uso di **deletejournal**

    L'eliminazione o la disattivazione di un journal delle modifiche attive è molto dispendiosa in termini di tempo, perché il sistema deve accedere a tutti i record nella tabella dei file master (MFT) e impostare l'ultimo attributo USN su 0 (zero). Questo processo può richiedere diversi minuti e può continuare dopo il riavvio del sistema, se è necessario un riavvio. Durante questo processo, il Journal delle modifiche non è considerato attivo, né è disabilitato. Quando il sistema sta disabilitando il Journal, non è possibile accedervi e tutte le operazioni Journal restituiscono errori. È necessario prestare particolare attenzione quando si disabilita un journal attivo, perché influisce negativamente sulle altre applicazioni che utilizzano il Journal.

## <a name="examples"></a><a name="BKMK_examples"></a>Esempi
Per creare un journal delle modifiche USN sull'unità C, digitare:

```
fsutil usn createjournal m=1000 a=100 c:
```

Per eliminare un journal delle modifiche Active USN sull'unità C, digitare:

```
fsutil usn deletejournal /d c:
```

Per abilitare il rilevamento dell'intervallo con una dimensione di blocco e una dimensione del file specificate, digitare:

```
fsutil usn enablerangetracking c=16384 s=67108864 C:
```

Per enumerare ed elencare le voci del journal delle modifiche tra due limiti specificati sull'unità C, digitare:

```
fsutil usn enumdata 1 0 1 c:
```

Per eseguire query sui dati USN per un volume sull'unità C, digitare:

```
fsutil usn queryjournal c:
```

Per leggere i dati USN per un file nella cartella \Temp sull'unità C, digitare:

```
fsutil usn readdata c:\temp\sample.txt
```

Per leggere il journal USN con un USN iniziale specifico, digitare:

```
fsutil usn readjournal startusn=0xF00
```

## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


