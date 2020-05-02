---
title: timeout
description: Argomento di riferimento per timeout, che consente di sospendere il processore dei comandi per il numero di secondi specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e26b4a84-0e30-46e1-aa10-0667b7d3cb4c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed66342c4f0bbe22e9d2dc6440d291941c769cd7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721353"
---
# <a name="timeout"></a>timeout

Sospende il processore dei comandi per il numero di secondi specificato.



## <a name="syntax"></a>Sintassi

```
timeout /t <TimeoutInSeconds> [/nobreak] 
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/t \<TimeoutInSeconds>|Specifica il numero decimale di secondi compreso tra -1 e 99999, di attesa prima che il processore dei comandi continua l'elaborazione. Il valore -1, il computer per un'attesa indefinita per una sequenza di tasti.|
|/NOBREAK|Specifica per ignorare le pressioni di tasti utente.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   Il **timeout** comando viene in genere utilizzato nei file batch.
-   Un utente preme un tasto riprende l'esecuzione del comando processore immediatamente, anche se non è scaduto il periodo di timeout.
-   Quando utilizzato in combinazione con il **sospensione** comando **timeout** è simile al **sospendere** comando.

## <a name="examples"></a>Esempi

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
