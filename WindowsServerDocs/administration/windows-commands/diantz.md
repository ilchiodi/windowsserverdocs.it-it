---
title: diantz
description: Argomento di riferimento per il comando diantz, che consente di raggruppare i file esistenti in un file CAB (con estensione cab).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 218ed5d7-1203-4d68-ad9b-65cdd022d54f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e45c0c4f71bc7faf6d5de0fa198ac872f6ff2597
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992496"
---
# <a name="diantz"></a>diantz

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprimere i file esistenti in un file cabinet (CAB). Questo comando esegue le stesse azioni del [comando makecab](makecab.md)aggiornato.

## <a name="syntax"></a>Sintassi

```
diantz [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
diantz [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<source>` | File da comprimere. |
| `<destination>` | Nome file da assegnare al file compresso. Se omesso, l'ultimo carattere del nome del file di origine viene sostituito con un carattere di sottolineatura (_) e utilizzato come destinazione. |
| /f `<directives_file>` | Un file con direttive **diantz** (può essere ripetuto). |
| /d var =`<value>` | Definisce una variabile con il valore specificato. |
| /l`<dir>` | Posizione in cui collocare destinazione (valore predefinito è la directory corrente). |
| /v[`<n>`] | Impostare il debug a livello di dettaglio (0 = none,..., 3 = full). |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Formato Microsoft Cabinet](https://docs.microsoft.com/previous-versions/bb417343(v=msdn.10))
