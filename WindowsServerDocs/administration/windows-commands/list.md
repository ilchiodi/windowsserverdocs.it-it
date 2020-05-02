---
title: list
description: Argomento di riferimento per il comando list, che visualizza un elenco di dischi, di partizioni in un disco, di volumi in un disco o di dischi rigidi virtuali (VHD).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 69b105a1-9710-4a06-8102-38cc9e475ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 53914468ddee4a8930fc05c677c94be700a89021
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724492"
---
# <a name="list"></a>list

Visualizza un elenco di dischi, di partizioni in un disco, di volumi in un disco o di dischi rigidi virtuali (VHD).

## <a name="syntax"></a>Sintassi

```
list { disk | partition | volume | vdisk }
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|disk|Visualizza un elenco di dischi e le relative informazioni, ad esempio le dimensioni, la quantità di spazio disponibile, il fatto che il disco sia un disco di base o dinamico e se il disco utilizza lo stile della partizione MBR (master boot record) o GPT (tabella di partizione GUID).|
|partition|Consente di visualizzare le partizioni elencate nella tabella delle partizioni del disco corrente.|
|volume|Visualizza un elenco di volumi di base e dinamici su tutti i dischi.|
|disco virtuale|Consente di visualizzare un elenco dei dischi rigidi virtuali collegati e/o selezionati. Questo comando Elenca i dischi rigidi virtuali scollegati se sono attualmente selezionate; Tuttavia, il tipo di disco è impostato su sconosciuto fino a quando non è collegato il disco rigido Virtuale. Il disco rigido virtuale contrassegnato con un asterisco (*) ha lo stato attivo.</br>Nota: questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.|

## <a name="remarks"></a>Osservazioni

-   Quando si elencano le partizioni su un disco dinamico, le partizioni potrebbero non corrispondere ai volumi dinamici sul disco. Questa discrepanza si verifica perché i dischi dinamici contengono voci nella tabella di partizione per il volume di sistema o il volume di avvio (se presente sul disco). Contengono inoltre una partizione che occupa la parte restante del disco per riservare spazio per i volumi dinamici.
-   L'oggetto contrassegnato con un asterisco (*) ha lo stato attivo.
-   Quando si elencano i dischi, se un disco risulta mancante, il numero di disco è preceduto da M. Ad esempio, il primo disco mancante è numerato M0.

## <a name="examples"></a>Esempi

```
list disk
list partition
list volume
list vdisk
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

