---
title: nslookup set root
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d913669fd4fede06c9983756df1bbf626ca430ac
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723574"
---
# <a name="nslookup-set-root"></a>nslookup set root

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica il nome del server principale per le query.
## <a name="syntax"></a>Sintassi
```
set root=<RootServer>
```
### <a name="parameters"></a>Parametri

|    Parametro    |                                   Descrizione                                    |
|-----------------|----------------------------------------------------------------------------------|
|  <RootServer>   | Specifica il nuovo nome per il server radice. Il valore predefinito è ns.nic.ddn.mil. |
| {Help &#124;?} |              Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.               |

## <a name="remarks"></a>Osservazioni
- Il sottocomando **set root** influiscono sul sottocomando **radice** .
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  - [Sintassi della riga di comando chiave](command-line-syntax-key.md)
  [nslookup radice](nslookup-root.md)
