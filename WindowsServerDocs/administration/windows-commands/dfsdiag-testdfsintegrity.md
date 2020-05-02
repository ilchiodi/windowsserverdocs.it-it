---
title: TestDFSIntegrity Dfsdiag
description: Argomento di riferimento per **Dfsdiag TestDFSIntegrity**, che controlla l'integrità dello spazio dei nomi file System distribuito (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21aa6ef3d7d4a7b4a9c64fc51aec77f49f1e0a0c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719574"
---
# <a name="dfsdiag-testdfsintegrity"></a>TestDFSIntegrity Dfsdiag

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica l'integrità dello spazio dei nomi file system distribuito (DFS) eseguendo i test seguenti:

- Verifica che le incoerenze tra i controller di dominio o danneggiamento dei metadati DFS.

- Convalida la configurazione dell'enumerazione basata sull'accesso per verificare che sia coerente tra i metadati DFS e la condivisione server dello spazio dei nomi.

- Rileva le cartelle DFS sovrapposte (collegamenti), le cartelle duplicate e le cartelle con destinazioni di cartelle sovrapposte.

## <a name="syntax"></a>Sintassi

```
dfsdiag /TestDFSIntegrity /DFSRoot: <DFS root path> [/Recurse] [/Full]
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
|-------|--------|
| /DFSRoot:`<DFS root path>`| Lo spazio dei nomi DFS per la diagnosi. |
| /Recurse | Esegue il test incluso che lo spazio dei nomi interlinks. |
| /Full | Verifica la coerenza degli ACL di condivisione e NTFS e della configurazione lato client in tutte le destinazioni di cartella. Verifica inoltre che la proprietà online sia impostata. |

## <a name="examples"></a>Esempi

```
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

-   [Dfsdiag](dfsdiag.md)


