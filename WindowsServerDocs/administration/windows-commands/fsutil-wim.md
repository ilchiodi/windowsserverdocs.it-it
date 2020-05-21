---
title: fsutil wim
description: Argomento di riferimento per il comando fsutil Wim, che fornisce funzioni per individuare e gestire i file supportati da Windows Image (WIM).
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 6c6ff819-f349-4aea-b0be-1f637f631736
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: db6a946eac59269d2bb4072c46552ac84366ed40
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436826"
---
# <a name="fsutil-wim"></a>fsutil wim

> Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10

Fornisce funzioni per individuare e gestire i file supportati dall'immagine Windows (WIM).

## <a name="syntax"></a>Sintassi

```
fsutil wim [enumfiles] <drive name> <data source>
fsutil wim [enumwims] <drive name>
fsutil wim [queryfile] <filename>
fsutil wim [removewim] <drive name> <data source>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| EnumFiles | Enumera i file supportati da WIM. |
| `<drive name>` | Specifica il nome dell'unità. |
| `<data source>` | Specifica l'origine dati. |
| enumwims | Enumera il backup dei file WIM. |
| queryfile | Esegue una query se il file è supportato da WIM e, in tal caso, Visualizza i dettagli sul file WIM. |
| `<filename>` | Specifica il nome del file. |
| removewim | Rimuove un file WIM dai file di backup. |

### <a name="examples"></a>Esempi

Per enumerare i file per l'unità C: dall'origine dati 0, digitare:

```
fsutil wim enumfiles C: 0
```

Per enumerare il backup dei file WIM per l'unità C:, digitare:

```
fsutil wim enumwims C:
```

Per verificare se un file è supportato da WIM, digitare:

```
fsutil wim C:\Windows\Notepad.exe
```

Per rimuovere il file WIM dai file di backup per il volume C: e l'origine dati 2, digitare:

```
fsutil wim removewims C: 2
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
