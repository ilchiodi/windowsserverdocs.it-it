---
title: label
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bbae8bdd-97d4-4566-9118-7c95aa07645f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7ccb86e2167682e1048161f2d5f5386a8b5cf6ed
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841174"
---
# <a name="label"></a>label



Crea, modifica o Elimina l'etichetta di volume (vale a dire il nome) del disco. Se utilizzata senza parametri, il **etichetta** comando Modifica l'etichetta di volume corrente oppure Elimina etichetta esistente.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
label [/mp] [<Volume>] [<Label>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/MP|Specifica che il volume deve essere trattato come un nome di volume o punto di montaggio.|
|\<volume >|Specifica una lettera di unità (seguita da due punti), punto di montaggio o il nome del volume. Se viene specificato un nome di volume, il **/mp** parametro non è necessario.|
|Etichetta \<>|Specifica l'etichetta per il volume.|
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

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)