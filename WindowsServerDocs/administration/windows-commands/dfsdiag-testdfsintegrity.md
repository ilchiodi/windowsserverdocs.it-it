---
title: testdfsintegrity Dfsdiag
description: Argomento di riferimento per il comando Dfsdiag testdfsintegrity, che controlla l'integrità dello spazio dei nomi file system distribuito (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b54c7f597926abc91bb9201dfec1a04f44e04ecb
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992964"
---
# <a name="dfsdiag-testdfsintegrity"></a>testdfsintegrity Dfsdiag

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica l'integrità dello spazio dei nomi file system distribuito (DFS) eseguendo i test seguenti:

- Verifica che le incoerenze tra i controller di dominio o danneggiamento dei metadati DFS.

- Convalida la configurazione dell'enumerazione basata sull'accesso per verificare che sia coerente tra i metadati DFS e la condivisione server dello spazio dei nomi.

- Rileva le cartelle DFS sovrapposte (collegamenti), le cartelle duplicate e le cartelle con destinazioni di cartelle sovrapposte.

## <a name="syntax"></a>Sintassi

```
dfsdiag /testdfsintegrity /DFSroot: <DFS root path> [/recurse] [/full]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /DFSroot:`<DFS root path>` | Lo spazio dei nomi DFS per la diagnosi. |
| /recurse | Esegue il test, inclusi eventuali intercollegamenti dello spazio dei nomi. |
| /Full | Verifica la coerenza della condivisione e degli ACL NTFS, insieme alla configurazione lato client su tutte le destinazioni di cartelle. Verifica inoltre che la proprietà online sia impostata. |

## <a name="examples"></a>Esempi

Per verificare l'integrità e la coerenza degli spazi dei nomi file system distribuito (DFS) in *contoso. com\MyNamespace*, inclusi eventuali Interlink, digitare:

```
dfsdiag /testdfsintegrity /DFSRoot:\contoso.com\MyNamespace /recurse /full
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Dfsdiag](dfsdiag.md)
