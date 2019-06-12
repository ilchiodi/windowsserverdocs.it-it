---
title: diskcomp
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f56f534-a356-4daa-8b4f-38e089341e42
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ccd1a347f9ac51fc98c963dedb1c0ab3fcd27d41
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439593"
---
# <a name="diskcomp"></a>diskcomp



Confronta il contenuto di due dischi floppy. Se utilizzata senza parametri, **verrà** Usa l'unità corrente per il confronto di entrambi i dischi. Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
diskcomp [<Drive1>: [<Drive2>:]]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Drive1>|Specifica l'unità contenente uno dei dischi floppy.|
|\<Drive2>|Specifica l'unità che contiene l'altro disco floppy.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

- Uso di dischi

  Il **verrà** comando funziona solo con i dischi floppy. Non è possibile usare **verrà** con un disco rigido. Se si specifica un'unità disco rigido per *unità1* oppure *unità2*, **verrà** Visualizza il messaggio di errore seguente:  
  ```
  Invalid drive specification
  Specified drive does not exist
  or is nonremovable
  ```  
- Confronto tra dischi

  Se tutte le tracce in due dischi di cui eseguire il confronto sono uguali **verrà** viene visualizzato il messaggio seguente:  
  ```
  Compare OK
  ```  
  Se le tracce non sono uguali **verrà** viene visualizzato un messaggio simile al seguente:  
  ```
  Compare error on
  side 1, track 2
  ```  
  Quando **verrà** viene completato il confronto, viene visualizzato il messaggio seguente:  
  ```
  Compare another diskette (Y/N)?
  ```  
  Se si preme Y **verrà** richiede di inserire il disco per il successivo confronto. Se si preme N **verrà** interrompe il confronto.

  Quando **verrà** rende il confronto, ignora il numero di volume del disco.
- Omettendo i parametri di unità

  Se si omette il *unità2* parametro **verrà** usi l'unità corrente per *unità2*. Se si omettono entrambi i parametri, unità **verrà** usi l'unità corrente per entrambi. Se l'unità corrente è identico *unità1*, **verrà** chiederà di scambiare dischi in base alle esigenze.
- Usando un'unità

  Se si specifica la stessa unità disco floppy per *unità1* e *unità2*, **verrà** vengono quindi confrontati utilizzando una sola unità e viene richiesto di inserire i dischi in base alle esigenze. Potrebbe essere necessario cambiare i dischi più volte, a seconda della capacità dei dischi e la quantità di memoria disponibile.
- Confronto tra diversi tipi di dischi

  **Verrà** non è possibile confrontare un disco con un disco di fronte-retro, né un disco con un disco doppia densità ad alta densità su un solo lato. Se il disco nel *unità1* non è dello stesso tipo del disco nelle *unità2*, **verrà** viene visualizzato il messaggio seguente:  
  ```
  Drive types or diskette types not compatible
  ```  
- Usando **verrà** con unità di rete e reindirizzata

  **Verrà** non funziona in un'unità di rete o su un disco creato per il **subst** comando. Se si prova a usare **verrà** con un'unità della ognuno di questi tipi **verrà** Visualizza il messaggio di errore seguente:  
  ```
  Invalid drive specification
  ```  
- Confronto di un disco originale con una copia

  Quando si usa **verrà** con un disco creato usando **copia**, **verrà** potrebbe essere visualizzato un messaggio simile al seguente:  
  ```
  Compare error on 
  side 0, track 0
  ```  
  Questo tipo di errore può verificarsi anche se i file nei dischi sono identici. Sebbene **copia** Duplica informazioni, non necessariamente posizionarlo nella stessa posizione sul disco di destinazione.
- La comprensione **verrà** codici di uscita

  Nella tabella seguente viene illustrato ogni codice di uscita.  

  |Codice di uscita|Descrizione|
  |---------|-----------|
  |0|I dischi sono uguali|
  |1|Sono state rilevate differenze|
  |3|Si è verificato un errore hardware|
  |4|Si è verificato un errore di inizializzazione|

  Per elaborare i codici di uscita restituiti da **verrà**, è possibile usare la variabile di ambiente ERRORLEVEL sulle **se** riga di comando in un file batch.

## <a name="BKMK_examples"></a>Esempi

Se il computer dispone di un'unica unità disco floppy (ad esempio, l'unità A) e si desidera confrontare due dischi, digitare:
```
diskcomp a: a:
```
**Verrà** chiede all'utente di inserire ogni disco, in base alle esigenze.

Nell'esempio seguente illustra come elaborare una **verrà** uscire dal codice in un file batch che utilizza la variabile di ambiente ERRORLEVEL sulle **se** della riga di comando:
```
rem Checkout.bat compares the disks in drive A and B 
echo off 
diskcomp a: b: 
if errorlevel 4 goto ini_error 
if errorlevel 3 goto hard_error 
if errorlevel 1 goto no_compare
if errorlevel 0 goto compare_ok 
:ini_error 
echo ERROR: Insufficient memory or command invalid 
goto exit 
:hard_error 
echo ERROR: An irrecoverable error occurred 
goto exit 
:break 
echo "You just pressed CTRL+C" to stop the comparison 
goto exit 
:no_compare 
echo Disks are not the same 
goto exit 
:compare_ok 
echo The comparison was successful; the disks are the same 
goto exit 
:exit
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
