---
title: defrag
description: Argomento di riferimento per il comando Defrag, che consente di individuare e consolidare i file frammentati nei volumi locali per migliorare le prestazioni del sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aaf1d1ac-996a-4282-9b4d-1e8245ff162c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf3ca6febfa07c7780b959389ff57fe4f3a0018b
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993153"
---
# <a name="defrag"></a>defrag

> Si applica a: Windows 10, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Individua e Consolida file frammentati nei volumi locali per migliorare le prestazioni del sistema.

Appartenenza al gruppo locale **amministratori** o gruppo equivalente, è il requisito minimo necessario per eseguire questo comando.

## <a name="syntax"></a>Sintassi

```
defrag <volumes> | /c | /e <volumes>    [/h] [/m [n]| [/u] [v]]
defrag <volumes> | /c | /e <volumes> /a [/h] [/m [n]| [/u] [v]]
defrag <volumes> | /c | /e <volumes> /x [/h] [/m [n]| [/u] [v]]
defrag <volume> [<parameters>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<volume>` | Specifica il percorso di punto di montaggio o lettera di unità del volume da deframmentare o l'analisi. |
| /a | Analizzare i volumi specificati. |
| /C | Eseguire l'operazione in tutti i volumi. |
| /d | Eseguire la deframmentazione tradizionale (impostazione predefinita). In un volume a livelli, tuttavia, la deframmentazione tradizionale viene eseguita solo sul livello di capacità. |
| /e | Eseguire l'operazione in tutti i volumi ad eccezione di quelli specificati. |
| /g | Ottimizzare i livelli di archiviazione nei volumi specificati. |
| /h | Eseguire l'operazione con priorità normale (valore predefinito è bassa). |
| /i [n] | L'ottimizzazione del livello viene eseguita per al massimo n secondi in ogni volume. |
| /k | Esegue il consolidamento delle piastre sui volumi specificati. |
| /l | Eseguire il ritaglio sui volumi specificati. |
| /m [n] | Eseguire l'operazione in ciascun volume in parallelo in background. Al massimo n thread ottimizzano i livelli di archiviazione in parallelo. |
| /o | Eseguire l'ottimizzazione corretta per ogni tipo di supporto. |
| /t | Tenere traccia di un'operazione già in corso nel volume specificato. |
| /U | Stampa lo stato di avanzamento dell'operazione sullo schermo. |
| /v | Stampare l'output dettagliato contenente le statistiche di frammentazione. |
| /x | Eseguire il consolidamento di spazio libero nel volume specificati. |
| /? | Consente di visualizzare queste informazioni della Guida. |

#### <a name="remarks"></a>Osservazioni

- Non è possibile deframmentare unità o volumi file system specifici, tra cui:

  - Volumi bloccati dal file system.

  - Volumi la file system contrassegnata come Dirty, che indica il possibile danneggiamento.<br>Prima di poter `chkdsk` deframmentare il volume o l'unità, è necessario eseguire. È possibile determinare se un volume è modificato utilizzando il `fsutil dirty` comando.

  - Unità di rete.

  - CD-ROM.

  - Volumi di file System non **NTFS**, **refs**, **FAT** o **FAT32**.

- Non è possibile pianificare la deframmentazione di un'unità SSD (Solid State Drive) o di un volume in un disco rigido virtuale (VHD) che risiede in un'unità SSD.

- Per eseguire questa procedura, è necessario essere un membro del gruppo Administrators nel computer locale oppure avere ricevuto in delega l'autorità appropriata. Se il computer fa parte di un dominio, è possibile che i membri del gruppo Domain Admins siano in grado di eseguire questa procedura. Come procedura di sicurezza consigliata, provare a utilizzare **RunAs** per eseguire questa procedura.

- Un volume deve avere almeno 15% spazio per **defrag** in modo completo e corretto. **Defrag** utilizza questo spazio come area di ordinamento per i frammenti di file. Se un volume è inferiore a 15% di spazio libero, **defrag** solo parzialmente la deframmentazione. Per aumentare lo spazio disponibile su un volume, eliminare i file non necessari o spostarli in un altro disco.

- Mentre **defrag** è l'analisi e la deframmentazione di un volume, viene visualizzato un cursore lampeggiante. Quando **defrag** termine dell'analisi e la deframmentazione del volume, Visualizza il report di analisi, il rapporto di deframmentazione o entrambi i report e quindi viene chiusa al prompt dei comandi.

- Per impostazione predefinita, **defrag** Visualizza un riepilogo dei report di analisi e deframmentazione se non si specifica il **/a** o **/v** parametri.

- È possibile inviare i report a un file di testo digitando **>** <em>nomefile. txt</em>, dove *filename. txt* è un nome file specificato. Ad esempio: `defrag volume /v > FileName.txt`

- Per interrompere il processo di deframmentazione, dalla riga di comando, premere **CTRL + C**.

- L'esecuzione del comando **Defrag** e dell'utilità di deframmentazione dischi si escludono a vicenda. Se si usa l'utilità di deframmentazione dischi per deframmentare un volume e si esegue il comando **Defrag** da una riga di comando, il comando **Defrag** ha esito negativo. Viceversa, se si esegue il comando **Defrag** e si apre l'utilità di deframmentazione dischi, le opzioni di deframmentazione nell'utilità di deframmentazione dischi non sono disponibili.

## <a name="examples"></a>Esempi

Per deframmentare il volume dell'unità C, fornendo lo stato di avanzamento e l'output dettagliato, digitare:

```
defrag c: /u /v
```

Per deframmentare i volumi di unità C e D in parallelo in background, digitare:

```
defrag c: d: /m
```

Per eseguire un'analisi di frammentazione di un volume montato nell'unità C e fornire lo stato di avanzamento, digitare:

```
defrag c: mountpoint /a /u
```

Per deframmentare tutti i volumi con priorità normale e fornire un output dettagliato, digitare:

```
defrag /c /h /v
```

## <a name="scheduled-task"></a>Attività pianificata

Il processo di deframmentazione esegue l'attività pianificata come attività di manutenzione, che in genere viene eseguita ogni settimana. In qualità di amministratore, è possibile modificare la frequenza con cui l'attività viene eseguita tramite l'app **Ottimizza unità** .

- Quando viene eseguito dall'attività pianificata, **Defrag** usa le linee guida dei criteri seguenti per le unità SSD:

  - **Processi di ottimizzazione tradizionali**. Include la **deframmentazione tradizionale**, ad esempio lo spostamento di file per renderli ragionevolmente contigui e **ritagliare**. Questa operazione viene eseguita una volta al mese. Tuttavia, se la **deframmentazione** e il **ritaglio** tradizionali vengono ignorati, l' **analisi** non viene eseguita.

  - Se si esegue manualmente la **deframmentazione** in un'unità SSD, tra le esecuzioni pianificate normalmente, l'esecuzione successiva dell'attività pianificata esegue l' **analisi** e il **ritaglio**, ma ignora la **deframmentazione tradizionale** su tale unità SSD.

  - Se si ignora l' **analisi**, non verrà visualizzata un' **Ultima** fase di esecuzione aggiornata nell'app **Ottimizza unità** . Per questo motivo, l' **ultima esecuzione** può essere fino a un mese prima.

  - Si potrebbe notare che l'attività pianificata non ha deframmentato tutti i volumi. Questo è in genere dovuto al fatto che:

    - Il processo non riattiverà il computer per l'esecuzione.

    - Il computer non è collegato. Il processo non verrà eseguito se il computer è alimentato a batteria.

    - Il computer è stato avviato (ripreso dall'inattività).

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [chkdsk](chkdsk.md)

- [fsutil](fsutil.md)

- [fsutil dirty](fsutil-dirty.md)

- [Optimize-volume PowerShell](https://docs.microsoft.com/powershell/module/storage/optimize-volume?view=win10-ps)
