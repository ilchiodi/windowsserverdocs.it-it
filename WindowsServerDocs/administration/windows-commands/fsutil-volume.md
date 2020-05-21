---
title: fsutil volume
description: Argomento di riferimento per il comando fsutil volume, che smonta un volume, oppure esegue una query sull'unità disco rigido per determinare la quantità di spazio libero attualmente disponibile sull'unità disco rigido o il file che utilizza un cluster specifico.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 0397c204-b3f8-4fd8-b71d-b7efb117766d
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 18671447664c47af48b4ca074aab823fd2b78625
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436836"
---
# <a name="fsutil-volume"></a>fsutil volume

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Smonta un volume oppure esegue una query sull'unità disco rigido per determinare la quantità di spazio libero attualmente disponibile nell'unità disco rigido o il file che usa un cluster specifico.

## <a name="syntax"></a>Sintassi

```
fsutil volume [allocationreport] <volumepath>
fsutil volume [diskfree] <volumepath>
fsutil volume [dismount] <volumepath>
fsutil volume [filelayout] <volumepath> <fileID>
fsutil volume [list]
fsutil volume [querycluster] <volumepath> <cluster> [<cluster>] … …
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| allocationreport | Visualizza informazioni sull'utilizzo dell'archiviazione in un determinato volume. |
| `<volumepath>` | Specifica la lettera di unità (seguita da due punti). |
| DiskFree | Esegue una query sull'unità disco rigido per determinare la quantità di spazio disponibile. |
| smontare | Smonta un volume. |
| filelayout | Visualizza i metadati NTFS per il file specificato. |
| `<fileID>` | Specifica l'ID del file. |
| list | Elenca tutti i volumi presenti nel sistema. |
| querycluster | Trova il file che usa un cluster specificato. È possibile specificare più cluster con il parametro **querycluster** . |
| `<cluster>` | Specifica il numero di cluster logico (LCN). |

### <a name="examples"></a>Esempi

Per visualizzare un report sui cluster allocati, digitare:

```
fsutil volume allocationreport C:
```

Per smontare un volume sull'unità C, digitare:

```
fsutil volume dismount c:
```

Per eseguire una query sulla quantità di spazio disponibile di un volume sull'unità C, digitare:

```
fsutil volume diskfree c:
```

Per visualizzare tutte le informazioni su uno o più file specificati, digitare:

```
fsutil volume C: *
fsutil volume C:\Windows
fsutil volume C: 0x00040000000001bf
```

Per elencare i volumi su disco, digitare:

```
fsutil volume list
```

Per trovare i file che usano i cluster, specificati dai numeri del cluster logico 50 e 0x2000, sull'unità C, digitare:

```
fsutil volume querycluster C: 50 0x2000
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [Funzionamento di NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc781134(v=ws.10))
