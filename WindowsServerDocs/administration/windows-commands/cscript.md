---
title: cscript
description: Argomento di riferimento per il comando cscript, che avvia uno script in modo che venga eseguito in un ambiente della riga di comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fba3cbca-594e-4663-bb22-4ee0f63a1ac6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bfba4b6a1c75183d58664e74da22bb7f8b866739
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993164"
---
# <a name="cscript"></a>cscript

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Avvia uno script da eseguire in un ambiente della riga di comando.

>[!IMPORTANT]
> L'esecuzione di questa attività non richiede credenziali amministrative. Pertanto, per garantire un livello di protezione ottimale, eseguire questa attività come utente senza credenziali amministrative.

## <a name="syntax"></a>Sintassi

```
cscript <scriptname.extension> [/b] [/d] [/e:<engine>] [{/h:cscript | /h:wscript}] [/i] [/job:<identifier>] [{/logo | /nologo}] [/s] [/t:<seconds>] [x] [/u] [/?] [<scriptarguments>]
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| scriptname. Extension | Specifica il percorso e il nome del file di script con estensione del nome file facoltativo. |
| / b | Specifica la modalità batch, che non visualizza gli avvisi, errori di script o richieste di input. |
| /d | Avvia il debugger. |
| /e:`<engine>` | Specifica il motore che viene utilizzato per eseguire lo script. |
| /h: cscript | Registra cscript. exe come host di script predefinito per l'esecuzione di script. |
| /h: WScript | Registra Wscript. exe come host di script predefinito per l'esecuzione di script. Questa è la modalità predefinita. |
| /i | Specifica la modalità interattiva, che visualizza gli avvisi, errori di script e richieste di input. Si tratta dell'impostazione predefinita e del contrario `/b`di. |
| /minuto<identifier> | Esegue il processo identificato dall' *identificatore* in un file di script. wsf. |
| /logo | Specifica che l'intestazione di Windows Script Host verrà visualizzata nella console prima dell'esecuzione dello script. Si tratta dell'impostazione predefinita e del contrario `/nologo`di. |
| /nologo | Specifica che l'intestazione di Windows Script Host non viene visualizzata prima dell'esecuzione dello script. |
| /s | Salva le opzioni della riga di comando corrente per l'utente corrente. |
| /t:<seconds> | Specifica il tempo massimo che esecuzione dello script (in secondi). È possibile specificare fino a 32.767 secondi. Il valore predefinito non è alcun limite di tempo. |
| /U | Specifica Unicode per l'input e output che viene reindirizzato dalla console. |
| /x | Avvia lo script nel debugger. |
| /? | Visualizza i parametri di comando disponibili e fornisce supporto per il loro utilizzo. Equivale a digitare **cscript. exe** senza parametri e senza script. |
| scriptarguments | Specifica gli argomenti passati allo script. Ogni argomento dello script deve essere preceduto da una barra**/**(). |

#### <a name="remarks"></a>Osservazioni

- Ogni parametro è facoltativo. Tuttavia, non è possibile specificare gli argomenti dello script senza specificare uno script. Se non si specifica uno script o qualsiasi argomento dello script, cscript. exe Visualizza la sintassi cscript. exe e le opzioni host valide.

- Il parametro **/t** impedisce l'esecuzione eccessiva di script impostando un timer. Quando il tempo di esecuzione supera il valore specificato, cscript interrompe il modulo di gestione di script e termina il processo.

- File di script di Windows è in genere una delle seguenti estensioni: wsf, vbs e. js. Windows Script Host può utilizzare i file di script wsf. Ciascun file può utilizzare più motori di script ed eseguire più processi.

- Se si fa doppio clic su un file di script con un'estensione senza associazione, viene visualizzata la finestra **di dialogo Apri con** . Selezionare Wscript o cscript, quindi selezionare **Usa sempre il programma per aprire questo tipo di file**. In questo modo, WScript. exe o cscript viene registrato come host di script predefinito per i file di questo tipo di file.

## <a name="additional-references"></a>Altri riferimenti

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
