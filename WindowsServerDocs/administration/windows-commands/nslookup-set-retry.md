---
title: nslookup set retry
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2b95c4c8af2d7960270fd43f7a766b313ddbc07a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838344"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il numero di tentativi.
## <a name="syntax"></a>Sintassi
```
set retry=<Number>
```
### <a name="parameters"></a>Parametri

|    Parametro    |                                      Descrizione                                       |
|-----------------|----------------------------------------------------------------------------------------|
|    <Number>     | Specifica il nuovo valore per il numero di tentativi. Il numero predefinito di tentativi è 4. |
| {Help &#124; ?} |                 Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                  |

## <a name="remarks"></a>Note
- Quando una risposta a una richiesta non viene ricevuta entro un determinato intervallo di tempo, il periodo di timeout viene raddoppiato e la richiesta viene inviata nuovamente. Il valore Retry controlla il numero di volte in cui una richiesta viene inviata di nuovo prima di rinunciare. È possibile modificare il periodo di timeout con il sottocomando **set timeout** .
  ## <a name="additional-references"></a>Altre informazioni di riferimento
  - [Chiave sintassi della riga di comando](command-line-syntax-key.md)
  [timeout set nslookup](nslookup-set-timeout.md)
