---
title: timeout
description: Argomento dei comandi di Windows per timeout, che consente di sospendere il processore dei comandi per il numero di secondi specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e26b4a84-0e30-46e1-aa10-0667b7d3cb4c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd0a43e49e8a7567ac975333b04a9e6f549a0fd8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832814"
---
# <a name="timeout"></a>timeout

Sospende il processore dei comandi per il numero di secondi specificato.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
timeout /t <TimeoutInSeconds> [/nobreak] 
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/t \<TimeoutInSeconds >|Specifica il numero decimale di secondi compreso tra -1 e 99999, di attesa prima che il processore dei comandi continua l'elaborazione. Il valore -1, il computer per un'attesa indefinita per una sequenza di tasti.|
|/NOBREAK|Specifica per ignorare le pressioni di tasti utente.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Il **timeout** comando viene in genere utilizzato nei file batch.
-   Un utente preme un tasto riprende l'esecuzione del comando processore immediatamente, anche se non è scaduto il periodo di timeout.
-   Quando utilizzato in combinazione con il **sospensione** comando **timeout** è simile al **sospendere** comando.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per sospendere il processore dei comandi per dieci secondi, digitare:
```
timeout /t 10
```
Per sospendere il processore dei comandi per 100 secondi e ignorare qualsiasi sequenza di tasti, digitare:
```
timeout /t 100 /nobreak
```
Per sospendere il processore dei comandi in modo indefinito finché non viene premuto un tasto, digitare:
```
timeout /t -1
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
