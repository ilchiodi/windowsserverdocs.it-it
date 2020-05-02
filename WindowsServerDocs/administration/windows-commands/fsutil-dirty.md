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
ms.openlocfilehash: 3b35938c21180199aabb74431d20a31167aea706
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725533"
---
# <a name="fsutil-dirty"></a>Fsutil dirty
> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Esegue una query o imposta il bit dirty di un volume. Quando un volume del dirty bit è impostato, **autochk** Controlla automaticamente il volume per gli errori al successivo riavvio del computer.



## <a name="syntax"></a>Sintassi

```
fsutil dirty {query | set} <VolumePath>
```

### <a name="parameters"></a>Parametri

|   Parametro   |                                                 Descrizione                                                  |
|---------------|--------------------------------------------------------------------------------------------------------------|
|     query     |                                  Esegue una query sul bit dirty del volume specificato.                                   |
|      set      |                                    Imposta il bit dirty del volume specificato.                                    |
| \<> VolumePath | Specifica il nome dell'unità seguito da due punti o GUID nel formato seguente: **volume {**<em>GUID</em>**}**. |

## <a name="remarks"></a>Osservazioni

-   Il bit dirty di un volume indica che lo stato del file system potrebbe essere incoerente. È possibile impostare il bit dirty perché:

    -   Il volume è online ed è in attesa di modifiche.

    -   Sono state apportate modifiche al volume e il computer è stato arrestato prima del commit delle modifiche nel disco.

    -   È stato rilevato un danneggiamento nel volume.

-   Se il bit dirty viene impostato al riavvio del computer, viene eseguito **chkdsk** per verificare l'integrità del file System e per tentare di risolvere eventuali problemi relativi al volume.

## <a name="examples"></a><a name="BKMK_examples"></a>Esempi
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

## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


