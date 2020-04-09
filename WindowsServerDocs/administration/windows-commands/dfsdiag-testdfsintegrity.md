---
title: TestDFSIntegrity Dfsdiag
description: Windows Commands Topic for **Dfsdiag TestDFSIntegrity**, che controlla l'integrità dello spazio dei nomi file System distribuito (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 714b79369898338a4e4a6e4fad8487709ab4fc60
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846274"
---
# <a name="dfsdiag-testdfsintegrity"></a>TestDFSIntegrity Dfsdiag

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
| /DFSRoot: `<DFS root path>`| Lo spazio dei nomi DFS per la diagnosi. |
| /Recurse | Esegue il test incluso che lo spazio dei nomi interlinks. |
| /Full | Verifica la coerenza degli ACL di condivisione e NTFS e della configurazione lato client in tutte le destinazioni di cartella. Verifica inoltre che la proprietà online sia impostata. |

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

```
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full
```

## <a name="additional-references"></a>Altre informazioni di riferimento

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

-   [Dfsdiag](dfsdiag.md)


