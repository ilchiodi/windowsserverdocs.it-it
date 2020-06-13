---
title: nslookup set root
description: Argomento di riferimento per il comando nslookup set root, che modifica il nome del server radice utilizzato per le query.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1271dbeb0381d01e70380bded82a94ba20163853
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721454"
---
# <a name="nslookup-set-root"></a>nslookup set root

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica il nome del server principale per le query.

> [!NOTE]
> Questo comando supporta il comando [radice nslookup](nslookup-root.md) .

## <a name="syntax"></a>Sintassi

```
set root=<rootserver>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ---------- | ---------- |
| `<rootserver>` | Specifica il nuovo nome per il server radice. Il valore predefinito è **ns.nic.DDN.mil**. |
| /? | Visualizza la guida al prompt dei comandi. |
| /help | Visualizza la guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [nslookup root](nslookup-root.md)
