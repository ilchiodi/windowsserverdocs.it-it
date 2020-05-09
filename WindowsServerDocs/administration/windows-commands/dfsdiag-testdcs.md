---
title: testdcs Dfsdiag
description: Argomento di riferimento per il comando Dfsdiag testdcs, che controlla la configurazione dei controller di dominio nel dominio specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0bbe47474f99edb1626e61a372b02090d3a45ee3
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992997"
---
# <a name="dfsdiag-testdcs"></a>testdcs Dfsdiag

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controlla la configurazione dei controller di dominio eseguendo i test seguenti su ogni controller di dominio nel dominio specificato:

- Verifica che il servizio spazio dei nomi file system distribuito (DFS) sia in esecuzione e che il tipo di avvio sia impostato su **automatico**.

- Verifica il supporto dei riferimenti per il sito per NETLOGON e SYSvol.

- Verifica la coerenza dell'associazione del sito per il nome host e indirizzo IP.

## <a name="syntax"></a>Sintassi

```
dfsdiag /testdcs [/domain:<domain_name>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /Domain`<domain_name>` | Nome del dominio da verificare. Questo parametro è facoltativo e, Il valore predefinito è il dominio locale a cui viene aggiunto l'host locale. |

## <a name="examples"></a>Esempi

Per verificare la configurazione dei controller di dominio nel dominio *contoso.com* , digitare:

```
dfsdiag /testdcs /domain:contoso.com
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Dfsdiag](dfsdiag.md)
