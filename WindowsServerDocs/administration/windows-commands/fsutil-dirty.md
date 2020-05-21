---
title: fsutil dirty
description: Argomento di riferimento per il comando fsutil dirty, che esegue una query o imposta il bit dirty di un volume.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 385a2a7c-d6bd-4f11-9c18-fca0413f9e97
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 72defd974177675f53e89fb8570f028580b7e167
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435866"
---
# <a name="fsutil-dirty"></a>fsutil dirty

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Esegue una query o imposta il bit dirty di un volume. Quando un volume del dirty bit è impostato, **autochk** Controlla automaticamente il volume per gli errori al successivo riavvio del computer.

## <a name="syntax"></a>Sintassi

```
fsutil dirty {query | set} <volumepath>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| query | Esegue una query sul bit dirty del volume specificato. |
| set | Imposta il bit dirty del volume specificato. |
| `<volumepath>` | Specifica il nome dell'unità seguito da due punti o GUID nel formato seguente: `volume{GUID}` . |

#### <a name="remarks"></a>Osservazioni

- Il bit dirty di un volume indica che lo stato del file system potrebbe essere incoerente. È possibile impostare il bit dirty perché:

    - Il volume è online ed è in attesa di modifiche.

    - Sono state apportate modifiche al volume e il computer è stato arrestato prima del commit delle modifiche nel disco.

    - È stato rilevato un danneggiamento nel volume.

- Se il bit dirty viene impostato al riavvio del computer, viene eseguito **chkdsk** per verificare l'integrità del file System e per tentare di risolvere eventuali problemi relativi al volume.

### <a name="examples"></a>Esempi

Per eseguire una query sul bit dirty sull'unità C, digitare:

```
fsutil dirty query c:
```

    If the volume is dirty, the following output displays:

    `Volume C: is dirty`

    If the volume isn't dirty, the following output displays:

    `Volume C: is not dirty`

Per impostare il bit dirty sull'unità C, digitare:

```
fsutil dirty set C:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
