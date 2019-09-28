---
title: defrag
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2d930e224ac1610b5e49cbf5701778bfb6f14b2e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378707"
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
|D|Eseguire la deframmentazione tradizionale (impostazione predefinita). In un volume a livelli, tuttavia, la deframmentazione tradizionale viene eseguita solo sul livello di capacità.|
|E|Eseguire l'operazione in tutti i volumi ad eccezione di quelli specificati.|
|G|Ottimizzare i livelli di archiviazione nei volumi specificati.|
|H|Eseguire l'operazione con priorità normale (valore predefinito è bassa).|
|I n|L'ottimizzazione del livello viene eseguita per al massimo n secondi in ogni volume.|
|K|Esegue il consolidamento delle piastre sui volumi specificati.|
|L|Eseguire il ritaglio sui volumi specificati.|
|M [n]|Eseguire l'operazione in ciascun volume in parallelo in background. Al massimo n thread ottimizzano i livelli di archiviazione in parallelo.|
|O|Eseguire l'ottimizzazione corretta per ogni tipo di supporto.|
|T|Tenere traccia di un'operazione già in corso nel volume specificato.|
|U|stampa lo stato di avanzamento dell'operazione sullo schermo.|
|V|stampa l'output dettagliato contenente le statistiche di frammentazione.|
|x|Eseguire il consolidamento di spazio libero nel volume specificati.|
|?|Consente di visualizzare queste informazioni della Guida.|

## <a name="remarks"></a>Note
- È in grado di deframmentare tipi specifici di unità o volumi di file system:
  -   È possibile deframmentare i volumi che ha bloccato il file system.
  -   È Impossibile deframmentare volumi che il file system è contrassegnato come modificato, che indica un possibile danneggiamento. È necessario eseguire **chkdsk** in un volume danneggiato prima di deframmentare. È possibile determinare se un volume è danneggiato, utilizzare il **fsutil** dirty comando di query. Per ulteriori informazioni su **chkdsk** e **fsutil** Dirty, vedere [riferimenti aggiuntivi](defrag.md#BKMK_additionalRef).
  -   È possibile deframmentare le unità di rete.
  -   Non è possibile deframmentare i cdROM.
  -   Non è possibile deframmentare file system volumi che non sono **NTFS**, **refs**, **FAT** o **FAT32**.
- Con Windows Server 2008 R2, Windows Server 2008 e Windows Vista, è possibile pianificare per deframmentare un volume. Tuttavia, non è possibile pianificare per deframmentare un volume su un disco rigido virtuale (VHD) che risiede in un'unità SSD o un solido stato unità SSD.
- Per eseguire questa procedura è necessario essere membri del gruppo Administrators nel computer locale oppure disporre della delega per l'autorità appropriata. Se il computer fa parte di un dominio, i membri del gruppo Domain Admins potrebbero essere in grado di eseguire questa procedura. Come procedura di sicurezza consigliata, provare a utilizzare **RunAs** per eseguire questa procedura.
- Un volume deve avere almeno 15% spazio per **defrag** in modo completo e corretto. **Defrag** utilizza questo spazio come area di ordinamento per i frammenti di file. Se un volume è inferiore a 15% di spazio libero, **defrag** solo parzialmente la deframmentazione. Per aumentare lo spazio disponibile su un volume, eliminare i file non necessari o spostarli in un altro disco.
- Mentre **defrag** è l'analisi e la deframmentazione di un volume, viene visualizzato un cursore lampeggiante. Quando **defrag** termine dell'analisi e la deframmentazione del volume, Visualizza il report di analisi, il rapporto di deframmentazione o entrambi i report e quindi viene chiusa al prompt dei comandi.
- Per impostazione predefinita, **defrag** Visualizza un riepilogo dei report di analisi e deframmentazione se non si specifica il **/a** o **/v** parametri.
- È possibile inviare i report in un file di testo digitando **>** <em>nomefile</em>, dove *nomefile* è un nome file specificato. Ad esempio: `defrag volume /v > FileName.txt`
- Per interrompere il processo di deframmentazione, dalla riga di comando, premere **CTRL + C**.
- L'esecuzione del comando **Defrag** e dell'utilità di deframmentazione dischi si escludono a vicenda. Se si usa l'utilità di deframmentazione dischi per deframmentare un volume e si esegue il comando **Defrag** da una riga di comando, il comando **Defrag** ha esito negativo. Viceversa, se si esegue il comando **Defrag** e si apre l'utilità di deframmentazione dischi, le opzioni di deframmentazione nell'utilità di deframmentazione dischi non sono disponibili.

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
L'attività pianificata di deframmentazione viene eseguita come un'attività di manutenzione e viene in genere pianificata per essere eseguita ogni settimana. L'amministratore può modificare la frequenza utilizzando l'applicazione **Ottimizza unità** .
- Quando viene eseguito dall'attività pianificata, **Defrag** presenta i criteri seguenti per le unità SSD:
   - La **deframmentazione tradizionale** , ovvero lo spostamento dei file per renderli ragionevolmente contigui, e il **ritaglio** viene eseguito solo una volta al mese.
   - Se vengono ignorate la **deframmentazione** e il **ritaglio** tradizionali, l' **analisi** non viene eseguita.
      - Se l'utente ha eseguito manualmente la **deframmentazione** in un'unità SSD, ad indicare 3 settimane dopo l'esecuzione dell'ultima attività pianificata, la successiva esecuzione pianificata dell'attività eseguirà l' **analisi** e il **ritaglio** , ignorando però la **deframmentazione tradizionale** .
   - Se l' **analisi** viene ignorata, l' **ultima esecuzione** in **Ottimizza unità** non verrà aggiornata.  Quindi, per le unità SSD l' **ultima esecuzione** in **Ottimizza unità** può essere un mese precedente.
- Questa attività di manutenzione potrebbe non deframmentare tutti i volumi, in alcuni casi perché questa attività esegue le operazioni seguenti:
   - Non riattiva il computer per eseguire la deframmentazione
   - Viene avviato solo se il computer è alimentato da AC e si arresta se il computer passa alla batteria
   - Arresta se il computer cessa di essere inattivo

## <a name="BKMK_additionalRef"></a>Riferimenti aggiuntivi
-   [chkdsk](chkdsk.md)
-   [fsutil](fsutil.md)
-   [fsutil dirty](fsutil-dirty.md)
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
