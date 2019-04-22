---
title: wscript
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 771c1231ee5379ec797f535505839de8671e32a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821302"
---
# <a name="wscript"></a>wscript



Windows Script Host offre un ambiente in cui gli utenti possono eseguire gli script in un'ampia gamma di linguaggi che usano un'ampia gamma di modelli a oggetti per eseguire attività.

## <a name="syntax"></a>Sintassi

```
wscript [<scriptname>] [/b] [/d] [/e:<engine>] [{/h:cscript|/h:wscript}] [/i] [/job:<identifier>] [{/logo|/nologo}] [/s] [/t:<number>] [/x] [/?] [<ScriptArguments>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|scriptname|Specifica il percorso e il nome del file di script.|
|/ b|Specifica la modalità batch, che non visualizza gli avvisi, errori di script o richieste di input. Questo è l'opposto del **/i**.|
|/d|Avvia il debugger.|
|/e|Specifica il motore che viene utilizzato per eseguire lo script. Ciò consente di eseguire gli script che utilizzano un'estensione personalizzata. Senza il parametro /e, è possibile eseguire solo script che utilizzano estensioni di file registrati. Ad esempio, se si prova a eseguire questo comando:<br>```cscript test.admin```<br>Si riceverà questo messaggio di errore: Errore di input: Nessun modulo di gestione di script per l'estensione di file ". Admin."<br>Uno dei vantaggi dell'uso di estensioni di file non standard è che protegge accidentalmente doppio clic su uno script ed eseguendo quanto realmente non desiderato per l'esecuzione. <br>Ciò non crea un'associazione permanente tra l'estensione del nome file .admin e VBScript. Ogni volta che si esegue uno script che usa un'estensione di nome file .admin, è necessario usare il parametro /e.|
|/h:cscript|Registra **cscript.exe** come host di scripting predefinito per l'esecuzione di script.|
|/h:wscript|Registra **wscript.exe** come host di scripting predefinito per l'esecuzione di script. Questo è il valore predefinito quando la **/h** opzione viene omessa.|
|/i|Specifica la modalità interattiva, che visualizza gli avvisi, errori di script e richieste di input.</br>Questo è il valore predefinito e l'opposto del **/b**.|
|/job:\<identifier>|Esegue il processo identificato da *identifier* in un **wsf** file di script.|
|/logo|Specifica che l'intestazione di Windows Script Host verrà visualizzata nella console prima dell'esecuzione dello script.</br>Questo è il valore predefinito e l'opposto del **/nologo**.|
|/nologo|Specifica che l'intestazione di Windows Script Host non viene visualizzata prima dell'esecuzione dello script. Questo è l'opposto del **/logo**.|
|/s|Salva le opzioni del prompt dei comandi corrente per l'utente corrente.|
|/t:\<number>|Specifica il tempo massimo che esecuzione dello script (in secondi). È possibile specificare un massimo di 32.767 secondi.</br>Il valore predefinito non è alcun limite di tempo.|
|/x|Avvia lo script nel debugger.|
|ScriptArguments|Specifica gli argomenti passati allo script. Ogni argomento dello script deve essere preceduto da una barra (/).|
|/?|Visualizza la Guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Per l'esecuzione di questa attività non è necessario disporre di credenziali amministrative. Per una sicurezza ottimale, è pertanto consigliabile eseguire questa attività con un account utente privo di credenziali amministrative.
-   Per aprire il prompt dei comandi, nella schermata **Start** digitare **cmd**, quindi fare clic su **prompt dei comandi**.
-   Ogni parametro è facoltativo. Tuttavia, è possibile specificare argomenti script senza specificare uno script. Se non si specifica uno script o dei relativi argomenti, **wscript.exe** consente di visualizzare i **impostazioni di Windows Script Host** nella finestra di dialogo è possibile usare per impostare le proprietà globali per tutti gli script che **wscript.exe** viene eseguito nel computer locale.
-   Il **/t** parametro impedisce l'esecuzione di un numero eccessivo di script impostando un timer. Quando il tempo supera il valore specificato, **wscript** interrompe il motore di script e termina il processo.
-   I file di script di Windows hanno in genere una delle seguenti estensioni di nome file: **wsf**, **vbs**, **js**.
-   Se si fa doppio clic su un file script con un'estensione che non ha alcuna associazione, il **Apri con** viene visualizzata la finestra di dialogo. Selezionare **wscript** oppure **cscript**, quindi selezionare **sempre questo programma consente di aprire questo tipo di file**. Si registrano **wscript.exe** oppure **cscript.exe** come host di scripting predefinito per i file di questo tipo di file.
-   È possibile impostare le proprietà dei singoli script. Visualizzare [Panoramica di Windows Script Host](https://technet.microsoft.com/library/cc738350(v=ws.10).aspx) per altre informazioni.
-   Windows Script Host può utilizzare **wsf** i file di script. Ciascuna **wsf** file può utilizzare più motori di script ed eseguire più processi.

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
