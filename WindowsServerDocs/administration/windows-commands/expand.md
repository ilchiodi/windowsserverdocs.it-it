---
title: expand
description: Argomento di riferimento per il comando Espandi, che consente di espandere uno o più file compressi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 66de0488-a0c4-40c2-9b03-e40c107ba343
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1204f3db338f835b47db03eab3d178544a6acc85
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819121"
---
# <a name="expand"></a>expand

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si espande uno o più file compressi. È anche possibile usare questo comando per recuperare i file compressi dai dischi di distribuzione.

Il comando **Espandi** può essere eseguito anche dalla Console ripristino Windows utilizzando parametri diversi. Per ulteriori informazioni, vedere [ambiente ripristino Windows (WinRE)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Sintassi

```
expand [/r] <source> <destination>
expand /r <source> [<destination>]
expand /i <source> [<destination>]
expand /d <source>.cab [/f:<files>]
expand <source>.cab /f:<files> <destination>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /r | File ridenominazioni espansi. |
| origine | Specifica i file da espandere. *Origine* può essere costituito da una lettera di unità e i due punti, un nome di directory, un nome di file o una combinazione di questi. È possibile utilizzare caratteri jolly (**&#42;** o **?**). |
| destination | Specifica in cui i file da espandere.<p>Se l' *origine* è costituita da più file e non si specifica **/r**, la *destinazione* deve essere una directory. *Destinazione* può essere costituito da una lettera di unità e i due punti, un nome di directory, un nome di file o una combinazione di questi. Specifica di destinazione `file | path` . |
| /i | Rinomina file espansi, ma ignora la struttura di directory. |
| /d | Visualizza un elenco dei file nel percorso di origine. Non espande o estrae i file. |
| /f`<files>` | Specifica i file in un file cabinet (CAB) che si desidera espandere. È possibile utilizzare caratteri jolly (**&#42;** o **?**). |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
