---
title: Dfsdiag
description: Argomento di riferimento per il comando Dfsdiag, che fornisce informazioni di diagnostica per gli spazi dei nomi DFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7e9e0de18b48a4233b950ad6aa8f1e450a99da62
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992832"
---
# <a name="dfsdiag"></a>Dfsdiag

Fornisce informazioni di diagnostica per gli spazi dei nomi DFS.

## <a name="syntax"></a>Sintassi

```
dfsdiag /testdcs [/domain:<domain name>]
dfsdiag /testsites </machine:<server name>| /DFSPath:<namespace root or DFS folder> [/recurse]> [/full]
dfsdiag /testdfsconfig /DFSRoot:<namespace>
dfsdiag /testdfsintegrity /DFSRoot:<DFS root path> [/recurse] [/full]
dfsdiag /testreferral /DFSpath:<DFS path to get referrals> [/full]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| [testdcs Dfsdiag](dfsdiag-testdcs.md) | Controlli di configurazione del controller di dominio. |
| [testsites Dfsdiag](dfsdiag-testsites.md) | Controlli del sito le associazioni. |
| [testdfsconfig Dfsdiag](dfsdiag-testdfsconfig.md) | Controlla la configurazione dello spazio dei nomi DFS. |
| [testdfsintegrity Dfsdiag](dfsdiag-testdfsintegrity.md) | Controlla l'integrità dello spazio dei nomi DFS. |
| [testreferral Dfsdiag](dfsdiag-testreferral.md) | Controlla le risposte di riferimento. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
