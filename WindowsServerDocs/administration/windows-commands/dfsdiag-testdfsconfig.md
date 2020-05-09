---
title: testdfsconfig Dfsdiag
description: Argomento di riferimento per il testdfsconfig Dfsdiag, che controlla la configurazione di uno spazio dei nomi file system distribuito (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 106aeeb9-ea79-4e6e-829c-eca06309bab2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d9490f35c2d509c83d9008aa87627bd3c55a875
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992988"
---
# <a name="dfsdiag-testdfsconfig"></a>testdfsconfig Dfsdiag

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica la configurazione di uno spazio dei nomi file system distribuito (DFS) eseguendo le azioni seguenti:

- Verifica che il servizio spazio dei nomi DFS sia in esecuzione e che il tipo di avvio sia impostato su **automatico** in tutti i server dello spazio dei nomi.

- Verifica che la configurazione del Registro di sistema DFS sia coerenza tra i server dello spazio dei nomi.

- Convalida le dipendenze seguenti nei server dello spazio dei nomi del cluster:

  - Dipendenza delle risorse di primo livello dello spazio dei nomi sulla stessa risorsa nome rete.

  - Dipendenza dalla risorsa nome risorsa indirizzo IP di rete.

  - Dipendenza delle risorse di primo livello dello spazio dei nomi sulla risorsa disco fisico.

## <a name="syntax"></a>Sintassi

```
dfsdiag /testdfsconfig /DFSroot:<namespace>
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /DFSroot:`<namespace>` | Spazio dei nomi (radice DFS) da diagnosticare. |

## <a name="examples"></a>Esempi

Per verificare la configurazione degli spazi dei nomi file system distribuito (DFS) in *contoso. com\MyNamespace*, digitare:

```
dfsdiag /testdfsconfig /DFSroot:\contoso.com\MyNamespace
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Dfsdiag](dfsdiag.md)
