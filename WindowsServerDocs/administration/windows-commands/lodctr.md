---
title: lodctr
description: Argomento di riferimento per il comando Lodctr, che consente di registrare o salvare il nome del contatore delle prestazioni e le impostazioni del registro di sistema in un file e designare servizi attendibili.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a849abd-6b31-4833-bc8a-306c05eca29a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 221737d68280dabf34c270fccff02071ebf9b5a2
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820181"
---
# <a name="lodctr"></a>lodctr

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di registrare o salvare di impostazioni del Registro di sistema e nome del contatore delle prestazioni in un file e definire servizi attendibili.

## <a name="syntax"></a>Sintassi

```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<filename>` | Specifica il nome del file di inizializzazione che registra le impostazioni del nome del contatore delle prestazioni e il testo esplicativo. |
| /s`<filename>` | Specifica il nome del file in cui vengono salvate le impostazioni del registro di sistema del contatore delle prestazioni e il testo esplicativo. |
| /r | Ripristina le impostazioni del registro di sistema e il testo esplicativo dalle impostazioni del registro di sistema correnti e dai file delle prestazioni memorizzati nella cache correlati al registro. |
| /r`<filename>` | Specifica il nome del file che ripristina le impostazioni del registro di sistema del contatore delle prestazioni e il testo esplicativo.<p>**Avviso:** Se si usa questo comando, si sovrascriveranno tutte le impostazioni del registro di sistema del contatore delle prestazioni e il testo esplicativo, sostituendolo con la configurazione definita nel file specificato. |
| /t:`<servicename>` | Indica il servizio `<servicename>` è attendibile. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Se le informazioni fornite contengono spazi, racchiudere il testo tra virgolette, ad esempio "nome file 1".

### <a name="examples"></a>Esempi

Per salvare le impostazioni del registro di sistema delle prestazioni correnti e il testo esplicativo in file *Perf BACKUP1. txt*, digitare:

```
lodctr /s:perf backup1.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
