---
title: nslookup set timeout
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8506511fc203f94d395851471f6a981ef0765928
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838274"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il numero iniziale di secondi di attesa di una risposta a una richiesta di ricerca.
## <a name="syntax"></a>Sintassi
```
set timeout=<Number>
```
### <a name="parameters"></a>Parametri

|    Parametro    |                                           Descrizione                                            |
|-----------------|--------------------------------------------------------------------------------------------------|
|    <Number>     | Specifica il numero di secondi di attesa di una risposta. Il numero predefinito di secondi di attesa è 5. |
| {Help &#124; ?} |                      Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                       |

## <a name="remarks"></a>Note
- Quando una risposta a una richiesta non viene ricevuta entro il periodo di tempo specificato, il timeout viene raddoppiato e la richiesta viene inviata di nuovo. È possibile utilizzare il comando **imposta tentativi** per controllare il numero di tentativi.
  ## <a name="examples"></a><a name=BKMK_examples></a>Esempi
  Nell'esempio seguente viene impostato il timeout per ottenere una risposta a 2 secondi:
  ```
  set timeout=2
  ```
  ## <a name="additional-references"></a>Altre informazioni di riferimento
  - [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [nuovo tentativo set nslookup](nslookup-set-retry.md)
