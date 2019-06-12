---
title: nslookup set timeout
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f6c8863d0a9330fd3a8499b0e6dbc802bd95022
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436513"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il numero di secondi di attesa per la risposta a una richiesta di ricerca iniziale.
## <a name="syntax"></a>Sintassi
```
set timeout=<Number>
```
## <a name="parameters"></a>Parametri

|    Parametro    |                                           Descrizione                                            |
|-----------------|--------------------------------------------------------------------------------------------------|
|    <Number>     | Specifica il numero di secondi di attesa di una risposta. Il numero predefinito di secondi di attesa è 5. |
| {help &#124; ?} |                      Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                       |

## <a name="remarks"></a>Note
- Quando una risposta a una richiesta non viene ricevuta entro il periodo di tempo specificato, viene raddoppiato il timeout e la richiesta viene inviata nuovamente. È possibile usare la **set tentativi** comando per controllare il numero di tentativi.
  ## <a name="BKMK_examples"></a>Esempi
  L'esempio seguente imposta il timeout per ottenere una risposta a 2 secondi:
  ```
  set timeout=2
  ```
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  [Chiave sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup set tentativi](nslookup-set-retry.md)
