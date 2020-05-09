---
title: makecab
description: Argomento di riferimento per il comando makecab, che consente di raggruppare i file esistenti in un file CAB (con estensione cab).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4da95297-c593-427b-9f76-2f389c46cbf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f3d5e778ee78d812a3ec8c3683b01e0b304a127e
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992343"
---
# <a name="makecab"></a>makecab

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprimere i file esistenti in un file cabinet (CAB). Questo comando esegue le stesse azioni del comando **diantz** .

## <a name="syntax"></a>Sintassi

```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<source>` | File da comprimere. |
| `<destination>` | Nome file da assegnare al file compresso. Se omesso, l'ultimo carattere del nome del file di origine viene sostituito con un carattere di sottolineatura (_) e utilizzato come destinazione. |
| /f `<directives_file>` | Un file con **makecab** direttive (può essere ripetuto). |
| /d var =`<value>` | Definisce una variabile con il valore specificato. |
| /l`<dir>` | Posizione in cui collocare destinazione (valore predefinito è la directory corrente). |
| /v[`<n>`] | Impostare il debug a livello di dettaglio (0 = none,..., 3 = full). |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Formato Microsoft Cabinet](https://docs.microsoft.com/previous-versions/bb417343(v=msdn.10))
