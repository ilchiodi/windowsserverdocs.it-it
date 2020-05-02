---
title: nslookup set retry
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1baeeaefedc211434f46bd0cfad713f093a873bf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723579"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il numero di tentativi.
## <a name="syntax"></a>Sintassi
```
set retry=<Number>
```
### <a name="parameters"></a>Parametri

|    Parametro    |                                      Descrizione                                       |
|-----------------|----------------------------------------------------------------------------------------|
|    <Number>     | Specifica il nuovo valore per il numero di tentativi. Il numero predefinito di tentativi è 4. |
| {Help &#124;?} |                 Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                  |

## <a name="remarks"></a>Osservazioni
- Quando una risposta a una richiesta non viene ricevuta entro un determinato intervallo di tempo, il periodo di timeout viene raddoppiato e la richiesta viene inviata nuovamente. Il valore Retry controlla il numero di volte in cui una richiesta viene inviata di nuovo prima di rinunciare. È possibile modificare il periodo di timeout con il sottocomando **set timeout** .
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  - [Sintassi della riga di comando chiave](command-line-syntax-key.md)
  [nslookup set timeout](nslookup-set-timeout.md)
