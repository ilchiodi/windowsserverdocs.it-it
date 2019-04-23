---
title: list
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 69b105a1-9710-4a06-8102-38cc9e475ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef9262a04f469f54e43cf3a83efe30fac7ad8580
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854672"
---
# <a name="list"></a>list



Visualizza un elenco di dischi, delle partizioni in un disco, dei volumi in un disco o dei dischi rigidi virtuali (VHD).

## <a name="syntax"></a>Sintassi

```
list { disk | partition | volume | vdisk }
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|disco|Visualizza un elenco di dischi e le relative informazioni, ad esempio la dimensione, la quantità di spazio disponibile, se il disco è un disco di base o dinamico e indica se il disco Usa il record di avvio principale (MBR) o lo stile di partizione GUID partizione GPT (tabella).|
|denominata|Consente di visualizzare le partizioni elencate nella tabella delle partizioni del disco corrente.|
|volume|Visualizza un elenco di volumi di base e dinamici su tutti i dischi.|
|disco virtuale|Visualizza un elenco di dischi rigidi virtuali che sono collegati e/o selezionati. Questo comando Elenca i dischi rigidi virtuali scollegati se sono attualmente selezionate; Tuttavia, il tipo di disco è impostato su sconosciuto fino a quando non è collegato il disco rigido Virtuale. Il disco rigido virtuale contrassegnato con un asterisco (*) ha lo stato attivo.</br>Nota: Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.|

## <a name="remarks"></a>Note

-   Quando si elencano le partizioni su un disco dinamico, le partizioni potrebbero non corrispondere ai volumi dinamici sul disco. Questa discrepanza è dovuto al fatto che i dischi dinamici contengono voci nella tabella di partizione per il volume di sistema o un volume di avvio (se presente sul disco). Contengono inoltre una partizione che occupa la parte restante del disco per riservare spazio per i volumi dinamici.
-   L'oggetto contrassegnato con un asterisco (*) ha lo stato attivo.
-   Quando si elencano i dischi, se un disco risulta mancante, il numero di disco è preceduto da M. Ad esempio, il primo disco mancante è numerato M0.

## <a name="BKMK_examples"></a>Esempi

```
list disk
list partition
list volume
list vdisk
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

