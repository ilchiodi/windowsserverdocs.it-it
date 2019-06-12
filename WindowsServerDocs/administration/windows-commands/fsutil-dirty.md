---
ms.assetid: 385a2a7c-d6bd-4f11-9c18-fca0413f9e97
title: Fsutil dirty
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: d8a5c4905991203a051fea360ed91c9b372f6993
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439081"
---
# <a name="fsutil-dirty"></a>Fsutil dirty
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Esegue una query o imposta i bit dirty del volume. Quando un volume del dirty bit è impostato, **autochk** Controlla automaticamente il volume per gli errori al successivo riavvio del computer.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
fsutil dirty {query | set} <VolumePath>
```

## <a name="parameters"></a>Parametri

|   Parametro   |                                                 Descrizione                                                  |
|---------------|--------------------------------------------------------------------------------------------------------------|
|     query     |                                  Esegue una query bit dirty del volume specificato.                                   |
|      set      |                                    Imposta i bit dirty del volume specificato.                                    |
| \<VolumePath> | Specifica il nome di unità seguito da una virgola o un GUID nel formato seguente: **Volume {** <em>GUID</em> **}** . |

## <a name="remarks"></a>Note

-   Bit dirty del volume indica che il file system può essere in uno stato incoerente. Bit dirty può essere impostato perché:

    -   Il volume è online e sono state apportate modifiche in sospeso.

    -   Sono state apportate modifiche al volume e il computer è stato arrestato prima che il commit delle modifiche al disco.

    -   Danneggiamento rilevato nel volume.

-   Se è impostato il dirty bit durante il riavvio del computer **chkdsk** viene eseguito per verificare l'integrità del file system e tentare di risolvere eventuali problemi con il volume.

## <a name="BKMK_examples"></a>Esempi
Per eseguire una query il dirty bit nell'unità C, digitare:

```
fsutil dirty query c:
```

-   Se il volume è stato modificato, viene visualizzato l'output seguente:

    `Volume C: is dirty`

-   Se il volume non è stato modificato, viene visualizzato l'output seguente:

    `Volume C: is not dirty`

Per impostare il bit dirty sull'unità C, digitare:

```
fsutil dirty set C:
```

#### <a name="additional-references"></a>Altri riferimenti
[Indicazioni generali sulla sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


