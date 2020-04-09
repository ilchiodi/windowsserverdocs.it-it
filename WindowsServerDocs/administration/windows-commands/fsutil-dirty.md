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
ms.openlocfilehash: cf3685bae9ed76ede4da6df244139437d92250c0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844334"
---
# <a name="fsutil-dirty"></a>Fsutil dirty
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Esegue una query o imposta il bit dirty di un volume. Quando un volume del dirty bit è impostato, **autochk** Controlla automaticamente il volume per gli errori al successivo riavvio del computer.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
fsutil dirty {query | set} <VolumePath>
```

### <a name="parameters"></a>Parametri

|   Parametro   |                                                 Descrizione                                                  |
|---------------|--------------------------------------------------------------------------------------------------------------|
|     query     |                                  Esegue una query sul bit dirty del volume specificato.                                   |
|      set      |                                    Imposta il bit dirty del volume specificato.                                    |
| \<VolumePath > | Specifica il nome dell'unità seguito da due punti o GUID nel formato seguente: **volume {** <em>GUID</em> **}** . |

## <a name="remarks"></a>Note

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

## <a name="additional-references"></a>Altre informazioni di riferimento
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


