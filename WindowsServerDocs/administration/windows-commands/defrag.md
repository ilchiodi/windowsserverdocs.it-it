---
title: defrag
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aaf1d1ac-996a-4282-9b4d-1e8245ff162c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6997e878b2bb7b77a5920ad7398ef7c2301cc8c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813192"
---
# <a name="defrag"></a>defrag

>Si applica a: Windows 10, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Individua e Consolida file frammentati nei volumi locali per migliorare le prestazioni del sistema.
Appartenenza al gruppo locale **amministratori** o gruppo equivalente, è il requisito minimo necessario per eseguire questo comando.

## <a name="syntax"></a>Sintassi
```
defrag <volumes> | /C | /E <volumes>    [/H] [/M [n]| [/U] [/V]]
defrag <volumes> | /C | /E <volumes> /A [/H] [/M [n]| [/U] [/V]]
defrag <volumes> | /C | /E <volumes> /X [/H] [/M [n]| [/U] [/V]]
defrag <volume> [/<Parameter>]*
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|`<volume>`|Specifica il percorso di punto di montaggio o lettera di unità del volume da deframmentare o l'analisi.|
|A|Analizzare i volumi specificati.|
|C|Eseguire l'operazione in tutti i volumi.|
|D|Eseguire la deframmentazione tradizionale (questo è il valore predefinito). In un volume a livelli, deframmentazione tradizionali viene eseguita solo nel livello di capacità.|
|E|Eseguire l'operazione in tutti i volumi ad eccezione di quelli specificati.|
|G|Ottimizzare i livelli di archiviazione nel volume specificati.|
|H|Eseguire l'operazione con priorità normale (valore predefinito è bassa).|
|I n|Ottimizzazione dei livelli verrà eseguite al massimo n secondi in ogni volume.|
|K|Eseguire il consolidamento i volumi specificati.|
|L|Eseguire retrim i volumi specificati.|
|M [n]|Eseguire l'operazione in ciascun volume in parallelo in background. Al massimo n thread ottimizzare i livelli di archiviazione in parallelo.|
|O|Eseguire l'ottimizzazione appropriato per ogni tipo di supporto.|
|T|Tenere traccia di un'operazione già in corso nel volume specificato.|
|U|stampa lo stato di avanzamento dell'operazione sullo schermo.|
|V|stampare l'output dettagliato contenente le statistiche di frammentazione.|
|x|Eseguire il consolidamento di spazio libero nel volume specificati.|
|?|Consente di visualizzare queste informazioni della Guida.|

## <a name="remarks"></a>Note
-   È in grado di deframmentare tipi specifici di unità o volumi di file system:
    -   È possibile deframmentare i volumi che ha bloccato il file system.
    -   È Impossibile deframmentare volumi che il file system è contrassegnato come modificato, che indica un possibile danneggiamento. È necessario eseguire **chkdsk** in un volume danneggiato prima di deframmentare. È possibile determinare se un volume è danneggiato, utilizzare il **fsutil** dirty comando di query. Per altre informazioni sulle **chkdsk** e **fsutil** dirty, vedere [riferimenti aggiuntivi](defrag.md#BKMK_additionalRef).
    -   È possibile deframmentare le unità di rete.
    -   È possibile deframmentare cdROMs.
    -   È possibile deframmentare i volumi di file system che non sono **NTFS**, **ReFS**, **Fat** oppure **Fat32**.
-   Con Windows Server 2008 R2, Windows Server 2008 e Windows Vista, è possibile pianificare per deframmentare un volume. Tuttavia, non è possibile pianificare per deframmentare un volume su un disco rigido virtuale (VHD) che risiede in un'unità SSD o un solido stato unità SSD.
-   Per eseguire questa procedura è necessario essere membri del gruppo Administrators nel computer locale oppure disporre della delega per l'autorità appropriata. Se il computer fa parte di un dominio, i membri del gruppo Domain Admins potrebbero essere in grado di eseguire questa procedura. Come procedura di sicurezza consigliata, è consigliabile usare **runas** per eseguire questa procedura.
-   Un volume deve avere almeno 15% spazio per **defrag** in modo completo e corretto. **la deframmentazione** Usa quest'area come area di ordinamento per i frammenti di file. Se un volume è inferiore a 15% di spazio libero, **defrag** solo parzialmente la deframmentazione. Per aumentare lo spazio disponibile su un volume, eliminare i file non necessari o spostarli in un altro disco.
-   Mentre **defrag** è l'analisi e la deframmentazione di un volume, viene visualizzato un cursore lampeggiante. Quando **defrag** termine dell'analisi e la deframmentazione del volume, Visualizza il report di analisi, il rapporto di deframmentazione o entrambi i report e quindi viene chiusa al prompt dei comandi.
-   Per impostazione predefinita, **defrag** Visualizza un riepilogo dei report di analisi e deframmentazione se non si specifica il **/a** o **/v** parametri.
-   È possibile inviare i report in un file di testo digitando **>** *FileName.txt*, dove *nomefile* è un nome file specificato. Ad esempio: `defrag volume /v > FileName.txt`
-   Per interrompere il processo di deframmentazione, dalla riga di comando, premere **CTRL + C**.
-   In esecuzione la **defrag** comando e utilità di deframmentazione dischi si escludono a vicenda. Se si usa l'utilità di deframmentazione dischi per deframmentare un volume e si esegue la **defrag** comando in una riga di comando, il **defrag** comando ha esito negativo. Viceversa, se si esegue la **defrag** comando e aprire Utilità di deframmentazione dischi, non sono disponibili le opzioni di utilità di deframmentazione dischi.

## <a name="BKMK_examples"></a>Esempi
Per deframmentare il volume dell'unità C, fornendo lo stato di avanzamento e l'output dettagliato, digitare:
```
defrag C: /U /V
```
Per deframmentare i volumi di unità C e D in parallelo in background, digitare:
```
defrag C: D: /M
```
Per eseguire un'analisi di frammentazione di un volume montato nell'unità C e fornire lo stato di avanzamento, digitare:
```
defrag C: mountpoint /A /U
```
Per deframmentare tutti i volumi con priorità normale e fornire un output dettagliato, digitare:
```
defrag /C /H /V
```

## <a name="BKMK_scheduledTask"></a>Attività pianificata
La deframmentazione pianificati dell'esecuzione dell'attività come attività di manutenzione e in genere pianificato per l'esecuzione ogni settimana. Amministratore può modificare la frequenza usando **Ottimizza unità** dell'applicazione.
- Quando viene eseguito dall'attività pianificata **defrag** ha criteri seguenti per le unità SSD:
   - **La deframmentazione Traditional** (vale a dire lo spostamento di file per renderli ragionevolmente contigue) e **retrim** viene eseguita solo una volta al mese.
   - Se entrambe **tradizionale defrag** e **retrim** vengono ignorate **analysis** non viene eseguito.
      - Se l'utente è stata eseguita **tradizionale defrag** manualmente in un'unità SSD, ad esempio 3 settimane dopo pianificata l'ultima esecuzione di attività, quindi verrà eseguita l'attività pianificata successiva eseguire **analysis** e **retrim** ma ignorare **tradizionale defrag** su tale unità SSD.
   - Se **analysis** viene ignorato e il **ultima esecuzione** memento nel **Ottimizza unità** non verrà aggiornato.  In questo caso per le unità SSD il **ultima esecuzione** temporale nella **Ottimizza unità** può essere precedente al mese.
- Questa attività di manutenzione potrebbe non deframmentare tutti i volumi, in alcuni casi poiché questa attività esegue le operazioni seguenti:
   - Non riattivare il computer per poter eseguire la deframmentazione
   - Viene avviato solo se il computer è alimentato da rete elettrica e viene interrotta se il computer passa all'alimentazione a batteria
   - Viene interrotta se nel computer cessa di essere inattivo

## <a name="BKMK_additionalRef"></a>Riferimenti aggiuntivi
-   [chkdsk](chkdsk.md)
-   [fsutil](fsutil.md)
-   [fsutil dirty](fsutil-dirty.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
