---
title: bcdedit
description: Argomento comandi di Windows per **bcdedit** -creare nuovi archivi, modificare gli archivi esistenti e aggiungere i parametri del menu di avvio.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9448f4461a089a93382ef8cd9e804b382fca27e4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382252"
---
# <a name="bcdedit"></a>bcdedit



I file dati configurazione di avvio (BCD) forniscono un archivio utilizzato per descrivere le applicazioni di avvio e le impostazioni dell'applicazione di avvio. Gli oggetti e gli elementi nell'archivio sostituiscono in modo efficace boot. ini.

BCDEdit è uno strumento da riga di comando per la gestione di archivi BCD. Può essere usato per diversi scopi, tra cui la creazione di nuovi archivi, la modifica di archivi esistenti, l'aggiunta di parametri del menu di avvio e così via. BCDEdit ha essenzialmente lo stesso scopo di Bootcfg. exe nelle versioni precedenti di Windows, ma con due miglioramenti principali:
-   Espone una gamma più ampia di parametri di avvio rispetto a bootcfg. exe.
-   Ha migliorato il supporto per gli script.

> [!NOTE]
> I privilegi amministrativi sono necessari per utilizzare BCDEdit per modificare BCD.

BCDEdit è lo strumento principale per modificare la configurazione di avvio di Windows Vista e versioni successive di Windows. È incluso nella distribuzione di Windows Vista nella cartella%WINDIR%\System32.

BCDEdit è limitato ai tipi di dati standard ed è progettato principalmente per eseguire singole modifiche comuni a BCD. Per operazioni più complesse o tipi di dati non standard, è consigliabile utilizzare l'Application Programming Interface (API) BCD Strumentazione gestione Windows (WMI) per creare strumenti personalizzati più potenti e flessibili.

## <a name="syntax"></a>Sintassi

```
BCDEdit /Command [<Argument1>] [<Argument2>] ...
```

## <a name="parameters"></a>Parametri

### <a name="general-bcdedit-command-line-option"></a>Opzione della riga di comando BCDEdit generale

|Opzione|Descrizione|
|------|-----------|
|/?|Visualizza un elenco di comandi BCDEdit. Se si esegue questo comando senza un argomento, viene visualizzato un riepilogo dei comandi disponibili. Per visualizzare la guida dettagliata per un particolare comando, eseguire **bcdedit/?** \<command >, dove <command> è il nome del comando a cui si sta cercando ulteriori informazioni. Ad esempio, **bcdedit/? createstore** Visualizza la guida dettagliata per il comando CreateStore.|

### <a name="parameters-that-operate-on-a-store"></a>Parametri che operano su un archivio

|Opzione|Descrizione|
|------|-----------|
|/CreateStore|Consente di creare un nuovo archivio dati di configurazione di avvio vuoto. L'archivio creato non è un archivio di sistema.|
|/Export|Esporta il contenuto dell'archivio di sistema in un file. Questo file può essere usato in un secondo momento per ripristinare lo stato dell'archivio di sistema. Questo comando è valido solo per l'archivio di sistema.|
|/Import|Ripristina lo stato dell'archivio di sistema utilizzando un file di dati di backup generato in precedenza utilizzando l'opzione **/Export** . Questo comando Elimina tutte le voci esistenti nell'archivio di sistema prima che l'importazione avvenga. Questo comando è valido solo per l'archivio di sistema.|
|/Store|Questa opzione può essere usata con la maggior parte dei comandi BCDedit per specificare l'archivio da usare. Se questa opzione non è specificata, BCDEdit opera nell'archivio di sistema. L'esecuzione del comando **bcdedit/store** è equivalente all'esecuzione del comando **bcdedit/enum Active** .|

### <a name="parameters-that-operate-on-entries-in-a-store"></a>Parametri che operano sulle voci in un archivio

|Parametro|Descrizione|
|---------|-----------|
|/Copy|Crea una copia di una voce di avvio specificata nello stesso archivio di sistema.|
|/Create|Crea una nuova voce nell'archivio dati di configurazione di avvio. Se viene specificato un identificatore noto, non è possibile specificare i parametri **/Application**, **/inherit**e **/Device** . Se un identificatore non è specificato o non è noto, è necessario specificare un'opzione **/Application**, **/inherit**o **/Device** .|
|/delete|Elimina un elemento da una voce specificata.|

### <a name="parameters-that-operate-on-entry-options"></a>Parametri che operano sulle opzioni di immissione

|Parametro|Descrizione|
|---------|-----------|
|/deletevalue|Elimina un elemento specificato da una voce di avvio.|
|/set|Imposta un valore dell'opzione di immissione.|

### <a name="parameters-that-control-output"></a>Parametri che controllano l'output

|Parametro|Descrizione|
|---------|-----------|
|/enum|Elenca le voci in un archivio. L'opzione **/enum** è il valore predefinito di BCEdit, pertanto l'esecuzione del comando **bcdedit** senza parametri equivale all'esecuzione del comando **bcdedit/enum Active** .|
|/v|Modalità dettagliata. In genere, tutti gli identificatori di voce noti sono rappresentati dal formato abbreviato descrittivo. Se si specifica **/v** come opzione della riga di comando, verranno visualizzati tutti gli identificatori completi. L'esecuzione del comando **bcdedit/v** è equivalente all'esecuzione del comando **bcdedit/enum Active/v** .|

### <a name="parameters-that-control-the-boot-manager"></a>Parametri che controllano il Boot Manager

|Parametro|Descrizione|
|---------|-----------|
|/bootsequence|Specifica un ordine di visualizzazione monouso da usare per l'avvio successivo. Questo comando è simile all'opzione **/displayorder** , ad eccezione del fatto che viene usato solo al successivo avvio del computer. Successivamente, viene ripristinato l'ordine di visualizzazione originale del computer.|
|/valore predefinito|Specifica la voce predefinita che il Boot Manager seleziona quando scade il timeout.|
|/displayorder|Specifica l'ordine di visualizzazione utilizzato dal gestore di avvio per la visualizzazione dei parametri di avvio per un utente.|
|/timeout|Specifica il tempo di attesa, in secondi, prima che il Boot Manager selezioni la voce predefinita.|
|/toolsdisplayorder|Specifica l'ordine di visualizzazione che deve essere utilizzato da Boot Manager quando viene visualizzato il menu **strumenti** .|

### <a name="parameters-that-control-emergency-management-services"></a>Parametri che controllano i servizi di gestione emergenze

|Parametro|Descrizione|
|---------|-----------|
|/bootems|Abilita o Disabilita i servizi di gestione emergenze (EMS) per la voce specificata.|
|/EMS|Abilita o Disabilita EMS per la voce di avvio del sistema operativo specificata.|
|/emssettings|Imposta le impostazioni globali di EMS per il computer. **/emssettings** non Abilita o Disabilita EMS per una voce di avvio particolare.|

### <a name="parameters-that-control-debugging"></a>Parametri che controllano il debug

|Parametro|Descrizione|
|---------|-----------|
|/bootdebug|Abilita o Disabilita il debugger di avvio per una voce di avvio specificata. Sebbene questo comando funzioni per qualsiasi voce di avvio, è efficace solo per le applicazioni di avvio.|
|/dbgsettings|Specifica o Visualizza le impostazioni globali del debugger per il sistema. Questo comando non Abilita o Disabilita il debugger del kernel. usare l'opzione **/debug** a tale scopo. Per impostare una singola impostazione del debugger globale, usare **bcdedit/set** \<dbgsettings > <type> <value> .|
|/debug|Abilita o Disabilita il debugger del kernel per una voce di avvio specificata.|

## <a name="examples"></a>Esempi

Per esempi di BCDEdit, vedere le informazioni di [riferimento sulle opzioni di bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcd-boot-options-reference).