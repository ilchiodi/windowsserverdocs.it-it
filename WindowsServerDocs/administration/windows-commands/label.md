---
title: etichetta
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bbae8bdd-97d4-4566-9118-7c95aa07645f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0c68fbbf3ea776bbf6cd49fc4fa446d5dd46542
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437916"
---
# <a name="label"></a>etichetta



Crea, modifica o Elimina l'etichetta di volume (vale a dire il nome) del disco. Se utilizzata senza parametri, il **etichetta** comando Modifica l'etichetta di volume corrente oppure Elimina etichetta esistente.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
label [/mp] [<Volume>] [<Label>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/MP|Specifica che il volume deve essere trattato come un nome di volume o punto di montaggio.|
|\<Volume >|Specifica una lettera di unità (seguita da due punti), punto di montaggio o il nome del volume. Se viene specificato un nome di volume, il **/mp** parametro non è necessario.|
|\<Label>|Specifica l'etichetta per il volume.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

- Windows consente di visualizzare l'etichetta di volume e il numero di serie (se presente) come parte dell'elenco di directory.
- Un'etichetta di volume NTFS può essere fino a 32 caratteri, inclusi gli spazi. Etichette di volume NTFS mantengono e visualizzare il case utilizzato al momento della creazione dell'etichetta.
- Se non si specifica un valore per il **etichetta** parametro, il **etichetta** comando Visualizza l'output nel formato seguente:  
  ```
  Volume in drive C: xxxxxxxxxxx 
  Volume Serial Number is xxxx-xxxx 
  Volume label (32 characters, ENTER for none)?
  ```  
  È possibile digitare una nuova etichetta di volume o premere INVIO per mantenere l'etichetta corrente. Se si preme INVIO e il volume dispone attualmente di un'etichetta, il **etichetta** comando viene visualizzato il messaggio seguente:  
  ```
  Delete current volume label (Y/N)?
  ```  
  Premere Y per eliminare l'etichetta, o premere N per mantenere l'etichetta.

## <a name="BKMK_examples"></a>Esempi

Per identificare un disco in unità a contenente informazioni sulle vendite di luglio, digitare:
```
label a:sales-july
```
Per eliminare l'etichetta corrente dell'unità C, seguire questi passaggi:
1. Al prompt dei comandi digitare:  
   ```
   Label
   ```  
   Deve essere visualizzato l'output simile al seguente:  
   ```
   Volume in drive C: is Main Disk
   Volume Serial Number is 6789-ABCD
   Volume label (32 characters, ENTER for none)?
   ```  
2. Premere INVIO. Viene visualizzato il seguente messaggio:  
   ```
   Delete current volume label (Y/N)?
   ```  
3. Premere Y per eliminare l'etichetta corrente.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)