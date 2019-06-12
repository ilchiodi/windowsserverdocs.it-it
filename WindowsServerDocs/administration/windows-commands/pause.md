---
title: Sospendi
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cab3afc3-d046-432f-a0bf-6282f0099032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5805fcc14d6874d95ba90537d72b560229ba99b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436315"
---
# <a name="pause"></a>Sospendi



Sospende l'elaborazione di un file batch e viene visualizzato il messaggio seguente:
```
Press any key to continue . . .
```
Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
pause
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

- Quando si esegue la **sospendere** comando, viene visualizzato il messaggio seguente:  
  ```
  Press any key to continue . . .
  ```  
- Se si preme CTRL + C per interrompere un programma batch, viene visualizzato il messaggio seguente:  
  ```
  Terminate batch job (Y/N)?
  ```  
  Se si preme Y (per Sì) in risposta a questo messaggio, il programma batch verrà interrotta e il controllo restituisce al sistema operativo.
- È possibile inserire il **sospendere** comando prima di una sezione del file batch che non è possibile elaborare. Quando **sospendere** sospende l'elaborazione del programma batch, è possibile premere CTRL + C e quindi premere Y per arrestare il programma batch.

## <a name="BKMK_examples"></a>Esempi

Per creare un file batch che chiede all'utente di cambiare i dischi in una delle unità, digitare:
```
@echo off 
:Begin 
copy a:*.* 
echo Put a new disk into drive A 
pause 
goto begin
```
In questo esempio, tutti i file su disco nell'unità vengono copiati nella directory corrente. Dopo che il messaggio viene richiesto di inserire un nuovo disco nell'unità, il **sospendere** comando sospende l'elaborazione in modo che è possibile modificare i dischi e quindi premere un tasto qualsiasi per riprendere l'elaborazione. Questo programma batch viene eseguito in un ciclo infinito, ovvero il **goto begin** comando Invia l'interprete dei comandi per l'etichetta di inizio del file batch. Per arrestare il programma batch, premere CTRL + C e quindi premere Y.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)