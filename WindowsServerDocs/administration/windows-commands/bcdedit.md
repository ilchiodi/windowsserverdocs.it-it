---
title: bcdedit
description: Argomento i comandi di Windows per **bcdedit** - creare nuovi archivi, modificare archivi esistenti e aggiungere parametri al menu di avvio.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ab2da47d-3aac-44a0-b7fd-bd9561d61553
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/27/2018
ms.openlocfilehash: c1ac016b299cbd72a406121c54762f4457b24286
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872442"
---
# <a name="bcdedit"></a>bcdedit



I file di dati di configurazione (BCD) di avvio offrono un archivio che viene usato per descrivere applicazioni di avvio e le impostazioni dell'applicazione di avvio. Gli oggetti e gli elementi nell'archivio di sostituiranno in modo efficace Boot. ini.

BCDEdit è uno strumento da riga di comando per la gestione di archivi BCD. Può essere utilizzato per diversi scopi, inclusa la creazione di nuovi archivi, gli archivi esistenti modificando, aggiunta di parametri di menu di avvio e così via. BCDEdit svolge essenzialmente la stessa funzione come Bootcfg.exe nelle versioni precedenti di Windows, ma con due miglioramenti principali:
-   Espone una vasta gamma di parametri di avvio rispetto Bootcfg.exe.
-   Ha migliorato il supporto di scripting.

> [!NOTE]
> Per utilizzare BCDEdit per modificare il file BCD sono necessari privilegi amministrativi.

BCDEdit è lo strumento principale per la modifica della configurazione di avvio di Windows Vista e versioni successive di Windows. È incluso con la distribuzione di Windows Vista nella cartella %Windir%\System32.

BCDEdit è limitato ai tipi di dati standard ed è progettato principalmente per eseguire singole modifiche comuni per BCD. Per le operazioni più complesse o tipi di dati non standard, prendere in considerazione tramite la Strumentazione gestione Windows (WMI) di BCD application programming interface (API) per creare più potenti e flessibili gli strumenti personalizzati.

## <a name="syntax"></a>Sintassi

```
BCDEdit /Command [<Argument1>] [<Argument2>] ...
```

## <a name="parameters"></a>Parametri

### <a name="general-bcdedit-command-line-option"></a>Opzione della riga di comando BCDEdit generale

|Opzione|Descrizione|
|------|-----------|
|/?|Visualizza un elenco di comandi BCDEdit. Questo comando senza un argomento, viene visualizzato un riepilogo dei comandi disponibili. Per visualizzare la Guida dettagliata per un particolare comando Esegui **bcdedit /?** \<comando >, dove <command> è il nome del comando si cercano altre informazioni su. Ad esempio, **bcdedit /? createstore** Visualizza la Guida dettagliata per il comando Createstore.|

### <a name="parameters-that-operate-on-a-store"></a>Parametri che operano su una Store

|Opzione|Descrizione|
|------|-----------|
|/createstore|Crea un nuovo archivio dati di configurazione di avvio vuoto. L'archivio creato non è un archivio di sistema.|
|/export|Esportazioni archivia il contenuto del sistema in un file. Questo file è utilizzabile in un secondo momento per ripristinare lo stato dell'archivio di sistema. Questo comando è valido solo per l'archivio di sistema.|
|/import|Ripristina lo stato dell'archivio di sistema usando un file di dati di backup generato in precedenza usando il **/Export** opzione. Questo comando Elimina tutte le voci esistenti nell'archivio di sistema prima che venga eseguita l'importazione. Questo comando è valido solo per l'archivio di sistema.|
|/store|Questa opzione è utilizzabile con la maggior parte dei comandi di BCDedit per specificare l'archivio da utilizzare. Se questa opzione non è specificata, BCDEdit opera in archivio di sistema. In esecuzione la **bcdedit /store** comandi da sola equivale all'esecuzione di **bcdedit /enum active** comando.|

### <a name="parameters-that-operate-on-entries-in-a-store"></a>Parametri che operano sulle voci in un Store

|Parametro|Descrizione|
|---------|-----------|
|/Copy|Crea una copia di una voce di avvio specificato nello stesso archivio di sistema.|
|/create|Crea una nuova voce nell'archivio dati di configurazione di avvio. Se viene specificato un identificatore noto, il **applicazione/**, **/ ereditano**, e **/device** non è possibile specificare i parametri. Se un identificatore non è specificato o non nota, un' **applicazione/**, **/ ereditano**, o **/device** opzione deve essere specificata.|
|/delete|Elimina un elemento da una voce specificata.|

### <a name="parameters-that-operate-on-entry-options"></a>Parametri che agiscono sulle opzioni di voce

|Parametro|Descrizione|
|---------|-----------|
|/deletevalue|Elimina un elemento specificato da una voce di avvio.|
|/set|Imposta un valore dell'opzione voce.|

### <a name="parameters-that-control-output"></a>Parametri che controllano l'Output

|Parametro|Descrizione|
|---------|-----------|
|/enum|Elenca le voci in un archivio. Il **/enum** opzione è il valore predefinito per BCEdit, pertanto l'esecuzione di **bcdedit** comando senza parametri è equivalente all'esecuzione di **bcdedit /enum active** comando.|
|/v|modalità dettagliata. In genere, gli identificatori di ingresso conosciuti sono rappresentati da loro sintassi abbreviata descrittivo. Che specifica **/v** come un'opzione della riga di comando consente di visualizzare tutti gli identificatori in modo completo. In esecuzione la **/v bcdedit** comandi da sola equivale all'esecuzione di **bcdedit /enum attivo /v** comando.|

### <a name="parameters-that-control-the-boot-manager"></a>Parametri che controllano il Boot Manager

|Parametro|Descrizione|
|---------|-----------|
|/bootsequence|Specifica un ordine di visualizzazione monouso da utilizzare per il prossimo avvio. Questo comando è simile al **/displayorder** opzione, ad eccezione del fatto che viene utilizzata solo la volta successiva che il computer viene avviato. Successivamente, il computer verrà ripristinato l'ordine di visualizzazione originale.|
|/default|Specifica la voce predefinita che consente di selezionare il boot manager quando il timeout scade.|
|/displayorder|Specifica l'ordine di visualizzazione del boot manager Usa la visualizzazione di parametri di avvio per un utente.|
|/timeout|Specifica il tempo di attesa, espresso in secondi, prima che il boot manager selezioni la voce predefinita.|
|/toolsdisplayorder|Specifica l'ordine di visualizzazione per la gestione di avvio da utilizzare quando si visualizzano le **strumenti** menu.|

### <a name="parameters-that-control-emergency-management-services"></a>Parametri che controllano i servizi di gestione emergenze

|Parametro|Descrizione|
|---------|-----------|
|/bootems|Abilita o disabilita i servizi di gestione emergenze (EMS) per la voce specificata.|
|/EMS|Abilita o disabilita EMS per la voce di avvio del sistema operativo specificato.|
|/emssettings|Imposta le impostazioni globali di EMS per il computer. **/emssettings** non abilitato o disabilitato EMS per qualsiasi voce di avvio specifico.|

### <a name="parameters-that-control-debugging"></a>Parametri che controllano il debug

|Parametro|Descrizione|
|---------|-----------|
|/bootdebug|Abilita o disabilita il debugger di avvio per una voce di avvio specificato. Anche se questo comando funziona per qualsiasi voce di avvio, è efficace solo per le applicazioni di avvio.|
|/dbgsettings|Specifica o Visualizza le impostazioni globali del debugger per il sistema. Questo comando non abilitare o disabilitare il debugger del kernel; usare la **/debug** opzione a tale scopo. Per impostare un'impostazione del debugger globale singoli, usare il **bcdedit /set** \<dbgsettings > <type> <value> .|
|/debug|Abilita o disabilita il debugger del kernel per una voce di avvio specificato.|

## <a name="examples"></a>Esempi

Per esempi di BCDEdit, vedere la [riferimento alle opzioni di BCDEdit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcd-boot-options-reference).