---
title: nslookup set timeout
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 32fcfcaeccb6599e9aaca21f9c085bb00857479c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372760"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il numero iniziale di secondi di attesa di una risposta a una richiesta di ricerca.
## <a name="syntax"></a>Sintassi
```
set timeout=<Number>
```
## <a name="parameters"></a>Parametri

|    Parametro    |                                           Descrizione                                            |
|-----------------|--------------------------------------------------------------------------------------------------|
|    <Number>     | Specifica il numero di secondi di attesa di una risposta. Il numero predefinito di secondi di attesa è 5. |
| {Help &#124; ?} |                      Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                       |

## <a name="remarks"></a>Note
- Quando una risposta a una richiesta non viene ricevuta entro il periodo di tempo specificato, il timeout viene raddoppiato e la richiesta viene inviata di nuovo. È possibile utilizzare il comando **imposta tentativi** per controllare il numero di tentativi.
  ## <a name="BKMK_examples"></a>Esempi
  Nell'esempio seguente viene impostato il timeout per ottenere una risposta a 2 secondi:
  ```
  set timeout=2
  ```
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup set di tentativi](nslookup-set-retry.md)
