---
title: replace
description: Informazioni su come usare il comando Sostituisci per sostituire i file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6143661e-d90f-4812-b265-6669b567dd1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4ac424154968b4f4c55664d0d20f524345b87986
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722377"
---
# <a name="replace"></a>replace



Sostituisce i file. Se utilizzato con il **/a** opzione **sostituire** aggiunge nuovi file in una directory invece di sostituire i file esistenti.



## <a name="syntax"></a>Sintassi

```
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/a] [/p] [/r] [/w] 
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/p] [/r] [/s] [/w] [/u] 
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[\<Unità1>:] [\<Path1>] \<> filename|Specifica il percorso e nome del file di origine o un set di file. *Filename* è obbligatorio e può includere caratteri jolly (**&#42;** e **?**).|
|[\<Unità2>:] [\<Path2>]|Specifica il percorso del file di destinazione. È possibile specificare un nome di file per file da sostituire. Se non si specifica un'unità o percorso, **sostituire** utilizza l'unità corrente e la directory come destinazione.|
|/a|Aggiunge nuovi file della directory di destinazione invece di sostituire i file esistenti. Non è possibile utilizzare questa opzione della riga di comando con il **/s** o **/u** opzione della riga di comando.|
|/ p|Chiede conferma prima di sostituire un file di destinazione o aggiunta di un file di origine.|
|/r|Sostituisce i file protetti e di sola lettura. Se si tenta di sostituire un file di sola lettura, ma non si specifica **/r**, risultati di un errore e arresta l'operazione di sostituzione.|
|/W|Attende che sia possibile inserire un disco prima di inizia la ricerca dei file di origine. Se non si specifica **/w**, **sostituire** inizia la sostituzione o l'aggiunta di file, subito dopo aver premuto INVIO.|
|/s|Cerca tutte le sottodirectory nella directory di destinazione e sostituisce i file corrispondenti. Non è possibile utilizzare **/s** con il **/a** opzione della riga di comando. Il **sostituire** comando non esegue la ricerca nelle sottodirectory specificate in *Path1*.|
|/U|Sostituisce solo i file nella directory di destinazione che sono meno recenti di quelli nella directory di origine. Non è possibile utilizzare **/u** con il **/a** opzione della riga di comando.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

- Come **sostituire** aggiunge o sostituisce i file, il file nomi vengono visualizzati sullo schermo. Dopo aver **sostituire** al termine, viene visualizzata una riga di riepilogo in uno dei seguenti formati:  
  ```
  nnn files added
  nnn files replaced
  no file added
  no file replaced
  ```  
- Se si utilizzano dischi floppy e si desidera cambiare i dischi durante la **sostituire** operazione, è possibile specificare il **/w** opzione della riga di comando in modo che **sostituire** attenderà di passare i dischi.
- Non è possibile utilizzare **sostituire** per aggiornare i file nascosti o i file di sistema.
- Nella tabella seguente viene illustrato ogni codice di uscita e una breve descrizione del relativo significato:  
  |Codice di uscita|Descrizione|
  |---------|-----------|
  |0|Il **sostituire** comando correttamente sostituiti o aggiunti i file.|
  |1|Il **sostituire** comando ha rilevato una versione non corretta di MS-DOS.|
  |2|Il **sostituire** comando non è riuscito a trovare i file di origine.|
  |3|Il **sostituire** comando non è riuscito a trovare il percorso di origine o destinazione.|
  |5|L'utente non ha accesso ai file che si desidera sostituire.|
  |8|Vi è memoria di sistema insufficiente per eseguire il comando.|
  |11|Viene utilizzata la sintassi errata nella riga di comando.|

> [!NOTE]
> È possibile utilizzare il parametro ERRORLEVEL sul **Se** riga di comando in un file batch per elaborare i codici di uscita restituiti da **sostituire**.

## <a name="examples"></a><a name="BKMK_examples"></a>Esempi

Per aggiornare tutte le versioni di un file denominato Telef (che vengono visualizzati in più directory sull'unità C), con la versione più recente del file Telef da un disco floppy nell'unità, digitare:

`replace a:\phones.cli c:\ /s`

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)