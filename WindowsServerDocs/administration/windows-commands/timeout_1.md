---
title: timeout
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e26b4a84-0e30-46e1-aa10-0667b7d3cb4c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 09f294eb78a8868b4e3962557a36199b69fae0c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385766"
---
# <a name="timeout"></a>timeout



Sospende il processore dei comandi per il numero di secondi specificato.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
timeout /t <TimeoutInSeconds> [/nobreak] 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/t \<TimeoutInSeconds >|Specifica il numero decimale di secondi compreso tra -1 e 99999, di attesa prima che il processore dei comandi continua l'elaborazione. Il valore -1, il computer per un'attesa indefinita per una sequenza di tasti.|
|/NOBREAK|Specifica per ignorare le pressioni di tasti utente.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Il **timeout** comando viene in genere utilizzato nei file batch.
-   Un utente preme un tasto riprende l'esecuzione del comando processore immediatamente, anche se non è scaduto il periodo di timeout.
-   Quando utilizzato in combinazione con il **sospensione** comando **timeout** è simile al **sospendere** comando.

## <a name="BKMK_examples"></a>Esempi

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

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
