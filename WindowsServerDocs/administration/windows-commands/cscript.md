---
title: cscript
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fba3cbca-594e-4663-bb22-4ee0f63a1ac6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef98a98088e345f267aa852318cee6e237604aa4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433996"
---
# <a name="cscript"></a>cscript

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

avvia uno script in modo che venga eseguito in un ambiente della riga di comando.
## <a name="syntax"></a>Sintassi
```
cscript <Scriptname.extension> [/B] [/D] [/E:<Engine>] [{/H:cscript|/H:wscript}] [/I] [/Job:<Identifier>] [{/Logo|/NoLogo}] [/S] [/T:<Seconds>] [/X] [/U] [/?] [<ScriptArguments>]
```
### <a name="parameters"></a>Parametri

|      Parametro       |                                                                      Descrizione                                                                       |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| NomeScript. estensione |                                 Specifica il percorso e il nome del file di script con estensione del nome file facoltativo.                                 |
|          /B          |                                Specifica la modalità batch, che non visualizza gli avvisi, errori di script o richieste di input.                                |
|          /D          |                                                                  Avvia il debugger.                                                                  |
|     /E:<Engine>      |                                                  Specifica il motore che viene utilizzato per eseguire lo script.                                                  |
|      /H:cscript      |                                         Registra cscript.exe come host di script predefinito per l'esecuzione di script.                                          |
|      /H:wscript      |                               Registra wscript.exe come host di script predefinito per l'esecuzione di script. Questa è l'impostazione predefinita.                               |
|          /I          |        Specifica la modalità interattiva, che visualizza gli avvisi, errori di script e richieste di input. Questo è il valore predefinito e l'opposto del **/b**.         |
|  /Job:<Identifier>   |                                             Esegue il processo identificato da *identificatore* in un file di script wsf.                                             |
|        / Logo         | Specifica che l'intestazione di Windows Script Host verrà visualizzata nella console prima dell'esecuzione dello script. Questo è il valore predefinito e l'opposto del **/Nologo**. |
|       /Nologo        |                                 Specifica che l'intestazione di Windows Script Host non viene visualizzata prima dell'esecuzione dello script.                                 |
|          /S          |                                             Salva le opzioni della riga di comando corrente per l'utente corrente.                                             |
|     /T:<Seconds>     |            Specifica il tempo massimo che esecuzione dello script (in secondi). È possibile specificare un massimo di 32.767 secondi. Il valore predefinito non è alcun limite di tempo.             |
|          /U          |                                      Specifica Unicode per l'input e output che viene reindirizzato dalla console.                                       |
|          /X          |                                                           Avvia lo script nel debugger.                                                           |
|          /?          |  Visualizza i parametri di comando disponibili e fornisce supporto per il loro utilizzo. Questo è lo stesso con cui si digita **cscript.exe** senza parametri e nessuno script.  |
|   ScriptArguments    |                        Specifica gli argomenti passati allo script. Ogni argomento dello script deve essere preceduto da una barra ( **/** ).                         |

### <a name="remarks"></a>Note
-   Per l'esecuzione di questa attività non è necessario disporre di credenziali amministrative. Per una sicurezza ottimale, è pertanto consigliabile eseguire questa attività con un account utente privo di credenziali amministrative.
-   Per aprire un prompt dei comandi, scegliere il **avviare** digitare **cmd**, quindi fare clic su **prompt dei comandi**.
-   Ogni parametro è facoltativo. Tuttavia, è possibile specificare argomenti script senza specificare uno script. Se non si specifica uno script o dei relativi argomenti, cscript.exe consente di visualizzare la sintassi cscript.exe e le opzioni di host valido.
-   Il **/T** parametro impedisce l'esecuzione di un numero eccessivo di script impostando un timer. Quando il tempo di esecuzione supera il valore specificato, cscript interrompe il motore di script e termina il processo.
-   File di script di Windows è in genere una delle seguenti estensioni: wsf, vbs e. js.
-   È possibile impostare le proprietà dei singoli script. Per ulteriori informazioni, vedere Argomenti correlati.
-   Windows Script Host può utilizzare i file di script wsf. Ciascun file può utilizzare più motori di script ed eseguire più processi.
-   Se si fa doppio clic su un file script con un'estensione che non ha alcuna associazione, il **aperta con** verrà visualizzata la finestra di dialogo. Selezionare cscript o wscript e quindi selezionare **sempre questo programma consente di aprire questo tipo di file**. Questo registra wscript.exe o cscript come host di script predefinito per i file di questo tipo di file.
-   È possibile impostare le proprietà dei singoli script. Visualizzare [riferimenti aggiuntivi](#BKMK_references) per altre informazioni.
-   Windows Script Host può utilizzare i file di script wsf. Ciascun file può utilizzare più motori di script ed eseguire più processi.

#### <a name="BKMK_references"></a>Riferimenti aggiuntivi

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
