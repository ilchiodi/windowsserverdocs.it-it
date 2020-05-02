---
title: Sospendi
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cab3afc3-d046-432f-a0bf-6282f0099032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 89e76c4f45f59c32ef879fb518a1a92c973f5cdf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723358"
---
# <a name="pause"></a>Sospendi



Sospende l'elaborazione di un programma batch e visualizza il messaggio seguente:
```
Press any key to continue . . .
```


## <a name="syntax"></a>Sintassi

```
pause
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

- Quando si esegue il comando **Sospendi** , viene visualizzato il messaggio seguente:  
  ```
  Press any key to continue . . .
  ```  
- Se si preme CTRL + C per arrestare un programma batch, viene visualizzato il messaggio seguente:  
  ```
  Terminate batch job (Y/N)?
  ```  
  Se si preme Y (per Sì) in risposta a questo messaggio, il programma batch termina e il controllo torna al sistema operativo.
- È possibile inserire il comando **pause** prima di una sezione del file batch che potrebbe non essere necessario elaborare. Quando **Sospendi sospende l'** elaborazione del programma batch, è possibile premere CTRL + C, quindi premere Y per arrestare il programma batch.

## <a name="examples"></a>Esempi

Per creare un programma batch che richiede all'utente di modificare i dischi in una delle unità, digitare:
```
@echo off 
:Begin 
copy a:*.* 
echo Put a new disk into drive A 
pause 
goto begin
```
In questo esempio, tutti i file sul disco nell'unità A vengono copiati nella directory corrente. Dopo **che il messaggio** richiede di inserire un nuovo disco nell'unità a, il comando Sospendi sospende l'elaborazione in modo che sia possibile modificare i dischi e quindi premere un tasto qualsiasi per riprendere l'elaborazione. Questo programma batch viene eseguito in un ciclo infinito: il comando **goto Begin** invia l'interprete dei comandi all'etichetta Begin del file batch. Per arrestare il programma batch, premere CTRL + C, quindi premere Y.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)