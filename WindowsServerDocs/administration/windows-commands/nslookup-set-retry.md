---
title: nslookup set retry
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 306bcc4f5e7ac98767c3c2e274100cf917874a8e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372864"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il numero di tentativi.
## <a name="syntax"></a>Sintassi
```
set retry=<Number>
```
## <a name="parameters"></a>Parametri

|    Parametro    |                                      Descrizione                                       |
|-----------------|----------------------------------------------------------------------------------------|
|    <Number>     | Specifica il nuovo valore per il numero di tentativi. Il numero predefinito di tentativi è 4. |
| {Help &#124; ?} |                 Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                  |

## <a name="remarks"></a>Osservazioni
- Quando una risposta a una richiesta non viene ricevuta entro un determinato intervallo di tempo, il periodo di timeout viene raddoppiato e la richiesta viene inviata nuovamente. Il valore Retry controlla il numero di volte in cui una richiesta viene inviata di nuovo prima di rinunciare. È possibile modificare il periodo di timeout con il sottocomando **set timeout** .
  ## <a name="additional-references"></a>riferimenti aggiuntivi
  [Chiave sintassi della riga di comando](command-line-syntax-key.md)
  [timeout set nslookup](nslookup-set-timeout.md)
