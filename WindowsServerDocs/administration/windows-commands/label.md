---
title: label
description: Argomento di riferimento per il comando label, che consente di creare, modificare o eliminare l'etichetta del volume, ovvero il nome, di un disco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bbae8bdd-97d4-4566-9118-7c95aa07645f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2d09328f79215c497bcb0ea4549b1f6ac227994
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817261"
---
# <a name="label"></a>label

Crea, modifica o Elimina l'etichetta di volume (vale a dire il nome) del disco. Se utilizzata senza parametri, il **etichetta** comando Modifica l'etichetta di volume corrente oppure Elimina etichetta esistente.

## <a name="syntax"></a>Sintassi

```
label [/mp] [<volume>] [<label>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /MP | Specifica che il volume deve essere trattato come un nome di volume o punto di montaggio. |
| `<volume>` | Specifica una lettera di unità (seguita da due punti), punto di montaggio o il nome del volume. Se viene specificato un nome di volume, il **/mp** parametro non è necessario. |
| `<label>` | Specifica l'etichetta per il volume. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="remarks"></a>Osservazioni

- Windows consente di visualizzare l'etichetta di volume e il numero di serie (se presente) come parte dell'elenco di directory.

- Un'etichetta di volume NTFS può essere fino a 32 caratteri, inclusi gli spazi. Etichette di volume NTFS mantengono e visualizzare il case utilizzato al momento della creazione dell'etichetta.

## <a name="examples"></a>Esempi

Per identificare un disco in unità a contenente informazioni sulle vendite di luglio, digitare:

```
label a:sales-july
```

Per visualizzare ed eliminare l'etichetta corrente per l'unità C, attenersi alla seguente procedura:

1. Al prompt dei comandi digitare:

   ```
   label
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

3. Premere **Y** per eliminare l'etichetta corrente oppure **N** se si desidera che l'etichetta esistente venga mantenuta.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)