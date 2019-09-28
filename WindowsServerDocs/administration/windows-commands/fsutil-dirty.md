---
ms.assetid: 385a2a7c-d6bd-4f11-9c18-fca0413f9e97
title: Fsutil dirty
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 01b5490ef7c57e48a43cae15902e03a33794a826
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377006"
---
# <a name="fsutil-dirty"></a>Fsutil dirty
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Esegue una query o imposta il bit dirty di un volume. Quando un volume del dirty bit è impostato, **autochk** Controlla automaticamente il volume per gli errori al successivo riavvio del computer.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
fsutil dirty {query | set} <VolumePath>
```

## <a name="parameters"></a>Parametri

|   Parametro   |                                                 Descrizione                                                  |
|---------------|--------------------------------------------------------------------------------------------------------------|
|     query     |                                  Esegue una query sul bit dirty del volume specificato.                                   |
|      set      |                                    Imposta il bit dirty del volume specificato.                                    |
| \<VolumePath > | Specifica il nome dell'unità seguito da due punti o GUID nel formato seguente: **Volume {** <em>GUID</em> **}** . |

## <a name="remarks"></a>Note

-   Il bit dirty di un volume indica che lo stato del file system potrebbe essere incoerente. È possibile impostare il bit dirty perché:

    -   Il volume è online ed è in attesa di modifiche.

    -   Sono state apportate modifiche al volume e il computer è stato arrestato prima del commit delle modifiche nel disco.

    -   È stato rilevato un danneggiamento nel volume.

-   Se il bit dirty viene impostato al riavvio del computer, viene eseguito **chkdsk** per verificare l'integrità del file System e per tentare di risolvere eventuali problemi relativi al volume.

## <a name="BKMK_examples"></a>Esempi
Per eseguire una query sul bit dirty sull'unità C, digitare:

```
fsutil dirty query c:
```

-   Se il volume è dirty, viene visualizzato l'output seguente:

    `Volume C: is dirty`

-   Se il volume non è dirty, viene visualizzato l'output seguente:

    `Volume C: is not dirty`

Per impostare il bit dirty sull'unità C, digitare:

```
fsutil dirty set C:
```

#### <a name="additional-references"></a>Altri riferimenti
[Indicazioni generali sulla sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


