---
title: fsutil tiering
description: Argomento di riferimento per il comando di suddivisione in livelli fsutil, che consente la gestione di funzioni del livello di archiviazione, ad esempio l'impostazione e la disabilitazione di flag ed elenco di livelli.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: e5f55f3e-8d2a-4526-8d67-36a539126c22
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 4fad2f5a7d868a38e187f49598235c40590e8eeb
ms.sourcegitcommit: ef089864980a1d4793a35cbf4cbdd02ce1962054
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2020
ms.locfileid: "84149764"
---
# <a name="fsutil-tiering"></a>fsutil tiering

> Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10

Consente la gestione delle funzioni del livello di archiviazione, ad esempio l'impostazione e la disabilitazione dei flag e l'elenco di livelli.

## <a name="syntax"></a>Sintassi

```
fsutil tiering [clearflags] <volume> <flags>
fsutil tiering [queryflags] <volume>
fsutil tiering [regionlist] <volume>
fsutil tiering [setflags] <volume> <flags>
fsutil tiering [tierlist] <volume>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| ClearFlags | Disabilita i flag di comportamento di suddivisione in livelli di un volume. |
| `<volume>` | Specifica il volume. |
| /trnh | Per i volumi con archiviazione a livelli, determina la disabilitazione della raccolta di calore.<p>Si applica solo a NTFS e ReFS. |
| queryflags | Esegue una query sui flag di comportamento di suddivisione in livelli di un volume. |
| area geografica | Elenca le aree a livelli di un volume e i rispettivi livelli di archiviazione. |
| SetFlags | Abilita i flag di comportamento di suddivisione in livelli di un volume. |
| suddivisione in livelli | Elenca i livelli di archiviazione associati a un volume. |

### <a name="examples"></a>Esempi

Per eseguire una query sui flag del volume C, digitare:

```
fsutil tiering queryflags C:
```

Per impostare i flag sul volume C, digitare:

```
fsutil tiering setflags C: /trnh
```

Per cancellare i flag sul volume C, digitare:

```
fsutil tiering clearflags C: /trnh
```

Per elencare le aree del volume C e i rispettivi livelli di archiviazione, digitare:

```
fsutil tiering regionlist C:
```

Per elencare i livelli del volume C, digitare:

```
fsutil tiering tierlist C:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
