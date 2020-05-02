---
title: nslookup set timeout
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68e9630b9690c9b6c9d4c316f8b328897289362c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723546"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il numero iniziale di secondi di attesa di una risposta a una richiesta di ricerca.
## <a name="syntax"></a>Sintassi
```
set timeout=<Number>
```
### <a name="parameters"></a>Parametri

|    Parametro    |                                           Descrizione                                            |
|-----------------|--------------------------------------------------------------------------------------------------|
|    <Number>     | Specifica il numero di secondi di attesa di una risposta. Il numero predefinito di secondi di attesa è 5. |
| {Help &#124;?} |                      Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                       |

## <a name="remarks"></a>Osservazioni
- Quando una risposta a una richiesta non viene ricevuta entro il periodo di tempo specificato, il timeout viene raddoppiato e la richiesta viene inviata di nuovo. È possibile utilizzare il comando **imposta tentativi** per controllare il numero di tentativi.
  ## <a name="examples"></a>Esempi
  Per impostare il timeout per ottenere una risposta a 2 secondi:
  ```
  set timeout=2
  ```
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  - [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup set Retry](nslookup-set-retry.md)
