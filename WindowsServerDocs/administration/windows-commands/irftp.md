---
title: irftp
description: Argomento di riferimento per il comando IrFTP, che invia file tramite un collegamento a infrarossi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e15c60a7-546d-4e9f-9871-43aaa1b569d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d19e4f0c325baf46c0a92bf04e39bc63863fe90
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818311"
---
# <a name="irftp"></a>irftp

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Invia i file su un collegamento a infrarossi.

> [!IMPORTANT]
> Assicurarsi che i dispositivi progettati per la comunicazione tramite un collegamento a infrarossi abbiano la funzionalità infrarossa abilitata e funzionino correttamente. Assicurarsi inoltre che tra i dispositivi venga stabilito un collegamento a infrarossi.

## <a name="syntax"></a>Sintassi

```
irftp [<drive>:\] [[<path>] <filename>] [/h][/s]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<drive>:\` | Specifica l'unità che contiene i file che si desiderano inviare tramite un collegamento a infrarossi. |
| `[path]<filename>` | Specifica il percorso e nome del file o set di file che si desiderano inviare tramite un collegamento a infrarossi. Se si specifica un set di file, è necessario specificare il percorso completo per ogni file. |
| /h | Specifica la modalità nascosta. Quando si utilizza tale modalità, i file vengono inviati senza visualizzare la finestra di dialogo Collegamento senza fili. |
| /s | Apre la finestra di dialogo **collegamento wireless** , in modo che sia possibile selezionare il file o il set di file che si desidera inviare senza utilizzare la riga di comando per specificare l'unità, il percorso e i nomi file. La finestra di dialogo **collegamento senza fili** viene visualizzata anche se si usa questo comando senza parametri. |

### <a name="examples"></a>Esempi

Per inviare *c:\EXAMPLE.txt* sul collegamento a infrarossi, digitare:

```
irftp c:\example.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
