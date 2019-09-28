---
title: wscript
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fbaf193-cdbd-414c-84c9-bb5720f84c29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 5e33f3f626ddb2645643ef3bfa8971667f692295
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361796"
---
# <a name="wscript"></a>wscript



Windows script host fornisce un ambiente in cui gli utenti possono eseguire script in un'ampia gamma di linguaggi che usano un'ampia gamma di modelli a oggetti per eseguire attività.

## <a name="syntax"></a>Sintassi

```
wscript [<scriptname>] [/b] [/d] [/e:<engine>] [{/h:cscript|/h:wscript}] [/i] [/job:<identifier>] [{/logo|/nologo}] [/s] [/t:<number>] [/x] [/?] [<ScriptArguments>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|scriptname|Specifica il percorso e il nome file del file di script.|
|/ b|Specifica la modalità batch, che non visualizza gli avvisi, errori di script o richieste di input. Si tratta dell'opposto di **/i**.|
|/d|Avvia il debugger.|
|/e|Specifica il motore che viene utilizzato per eseguire lo script. In questo modo è possibile eseguire script che usano un'estensione di file personalizzata. Senza il parametro/e, è possibile eseguire solo script che utilizzano estensioni di file registrate. Ad esempio, se si tenta di eseguire questo comando:<br>```cscript test.admin```<br>Verrà visualizzato il messaggio di errore seguente: Errore di input: Nessun motore di script per l'estensione di file ". admin".<br>Un vantaggio derivante dall'utilizzo di estensioni di file non standard consiste nel fatto che protegge accidentalmente facendo doppio clic su uno script ed eseguendo qualcosa che non si desidera eseguire. <br>Questa operazione non crea un'associazione permanente tra l'estensione del nome di file. admin e VBScript. Ogni volta che si esegue uno script che utilizza un'estensione di file con estensione admin, sarà necessario utilizzare il parametro/e.|
|/h: cscript|Registra **cscript. exe** come host di script predefinito per l'esecuzione di script.|
|/h: WScript|Registra **WScript. exe** come host di script predefinito per l'esecuzione di script. Si tratta dell'impostazione predefinita quando si omette l'opzione **/h** .|
|/i|Specifica la modalità interattiva, che visualizza gli avvisi, errori di script e richieste di input.</br>Si tratta dell'impostazione predefinita e l'opposto di **/b**.|
|/minuto: > \<identifier|Esegue il processo identificato dall' *identificatore* in un file di script **. wsf** .|
|/logo|Specifica che l'intestazione di Windows Script Host verrà visualizzata nella console prima dell'esecuzione dello script.</br>Si tratta dell'impostazione predefinita e del contrario di **/nologo**.|
|/nologo|Specifica che l'intestazione di Windows Script Host non viene visualizzata prima dell'esecuzione dello script. Si tratta del contrario di **/logo**.|
|/s|Salva le opzioni del prompt dei comandi correnti per l'utente corrente.|
|/t: \<number >|Specifica il tempo massimo che esecuzione dello script (in secondi). È possibile specificare fino a 32.767 secondi.</br>Il valore predefinito non è alcun limite di tempo.|
|/x|Avvia lo script nel debugger.|
|ScriptArguments|Specifica gli argomenti passati allo script. Ogni argomento dello script deve essere preceduto da una barra (/).|
|/?|Visualizza la Guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Per l'esecuzione di questa attività non è necessario disporre di credenziali amministrative. Per una sicurezza ottimale, è pertanto consigliabile eseguire questa attività con un account utente privo di credenziali amministrative.
-   Per aprire il prompt dei comandi, nella schermata **Start** digitare **cmd**, quindi fare clic su **prompt dei comandi**.
-   Ogni parametro è facoltativo. Tuttavia, è possibile specificare argomenti script senza specificare uno script. Se non si specifica alcuno script o argomenti dello script, **WScript. exe** Visualizza la finestra di dialogo **impostazioni di Windows script host** che consente di impostare le proprietà di scripting globali per tutti gli script eseguiti da **WScript. exe** nel computer locale.
-   Il parametro **/t** impedisce l'esecuzione eccessiva di script impostando un timer. Quando il tempo supera il valore specificato, **WScript** interrompe il modulo di gestione di script e termina il processo.
-   I file script di Windows in genere hanno una delle estensioni di file seguenti: **. wsf**, **. vbs**,. **JS**.
-   Se si fa doppio clic su un file script con un'estensione che non ha alcuna associazione, il **Apri con** viene visualizzata la finestra di dialogo. Selezionare **WScript** o **cscript**, quindi selezionare **Usa sempre il programma per aprire questo tipo di file**. In questo modo, **WScript. exe** o **cscript. exe** viene registrato come host di script predefinito per i file di questo tipo di file.
-   È possibile impostare le proprietà dei singoli script. Per ulteriori informazioni, vedere [Panoramica di Windows script host](https://technet.microsoft.com/library/cc738350(v=ws.10).aspx) .
-   Windows script host può usare i file di script **. wsf** . Ogni file con **estensione wsf** può utilizzare più motori di script ed eseguire più processi.

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
