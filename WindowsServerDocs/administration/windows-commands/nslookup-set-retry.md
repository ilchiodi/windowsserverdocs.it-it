---
title: nslookup set retry
description: Argomento di riferimento per il comando nslookup set Retry, che imposta il numero di tentativi di ottenere informazioni da un server specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 268a9f0023c0e7e19e8ed413895f639444fe3b88
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721464"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se una risposta non viene ricevuta entro un determinato intervallo di tempo, il periodo di timeout viene raddoppiato e la richiesta viene inviata nuovamente. Questo comando imposta il numero di volte in cui una richiesta viene inviata a un server per ottenere informazioni, prima di rinunciare.

> [!NOTE]
> Per modificare l'intervallo di tempo prima del timeout della richiesta, utilizzare il comando [nslookup set timeout](nslookup-set-timeout.md) .

## <a name="syntax"></a>Sintassi

```
set retry=<number>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ---------- | ---------- |
| `<number>` | Specifica il nuovo valore per il numero di tentativi. Il numero predefinito di tentativi è **4**. |
| /? | Visualizza la guida al prompt dei comandi. |
| /help | Visualizza la guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [nslookup set timeout](nslookup-set-timeout.md)
