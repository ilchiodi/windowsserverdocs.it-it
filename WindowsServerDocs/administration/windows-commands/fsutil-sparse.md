---
title: fsutil sparse
description: Argomento di riferimento per il comando fsutil sparse, che consente di gestire i file sparse.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 77545920-2d13-4f35-a4d1-14dbec8340dc
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: e68ac844bb7aa7e22a9df0ddb0c982b3701231d7
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435716"
---
# <a name="fsutil-sparse"></a>fsutil sparse

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Gestisce i file sparse. Un file sparse è un file con una o più aree di dati non allocati.

Un programma visualizza queste aree non allocate come contenenti byte con un valore zero e che non è presente spazio su disco che rappresenta questi zeri. Quando viene letto un file sparse, i dati allocati vengono restituiti come archiviati e vengono restituiti dati non allocati, per impostazione predefinita, come zeri, in base alla specifica del requisito di sicurezza C2. Supporto file sparse consente di deallocare da ovunque nel file di dati.

## <a name="syntax"></a>Sintassi

```
fsutil sparse [queryflag] <filename>
fsutil sparse [queryrange] <filename>
fsutil sparse [setflag] <filename>
fsutil sparse [setrange] <filename> <beginningoffset> <length>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| queryflag | Query sparse. |
| queryrange | Analizza un file e cerca gli intervalli che possono contenere dati diversi da zero. |
| setflag | Contrassegna il file indicato come sparse. |
| SetRange | Riempie un intervallo specificato di un file con zeri. |
| `<filename>` | Specifica il percorso completo del file, inclusi il nome file e l'estensione, ad esempio *C:\documents\filename.txt*. |
| `<beginningoffset>` | Specifica l'offset nel file da contrassegnare come sparse. |
| `<length>` | Specifica la lunghezza dell'area nel file da contrassegnare come sparse (in byte). |

#### <a name="remarks"></a>Osservazioni

- Vengono allocati tutti i dati significativi o diversi da zero, mentre tutti i dati non significativi (stringhe di grandi dimensioni di dati costituiti da zeri) non vengono allocati.

- In un file sparse, i grandi intervalli di zeri potrebbero non richiedere l'allocazione dei dischi. Lo spazio per i dati diversi da zero viene allocato in base alle esigenze quando il file viene scritto.

- Solo i file compressi o sparse possono avere intervalli azzerati noti al sistema operativo.

- Se il file è di tipo sparse o compresso, NTFS potrebbe deallocare spazio su disco all'interno del file. Questo consente di impostare l'intervallo di byte su zero senza estendere le dimensioni del file.

### <a name="examples"></a>Esempi

Per contrassegnare un file denominato *Sample. txt* nella directory *c:\Temp* come sparse, digitare:

```
fsutil sparse setflag c:\temp\sample.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
