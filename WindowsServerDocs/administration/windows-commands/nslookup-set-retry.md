---
title: nslookup set retry
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 876d8332e778aa0b3049354a21fbe01adb883729
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436656"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il numero di tentativi.
## <a name="syntax"></a>Sintassi
```
set retry=<Number>
```
## <a name="parameters"></a>Parametri

|    Parametro    |                                      Descrizione                                       |
|-----------------|----------------------------------------------------------------------------------------|
|    <Number>     | Specifica il nuovo valore per il numero di tentativi. Il numero di tentativi predefinito è 4. |
| {help &#124; ?} |                 Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                  |

## <a name="remarks"></a>Note
- Quando una risposta a una richiesta non viene ricevuta entro un determinato periodo di tempo, il periodo di timeout è raddoppiato negli anni e la richiesta viene inviata di nuovo. Il valore di ripetizione dei tentativi determina il numero di volte una richiesta viene inviata di nuovo prima di rinunciare. È possibile modificare il periodo di timeout con i **impostare il timeout** sottocomando.
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  [Chiave sintassi della riga di comando](command-line-syntax-key.md)
  [nslookup impostare timeout](nslookup-set-timeout.md)
