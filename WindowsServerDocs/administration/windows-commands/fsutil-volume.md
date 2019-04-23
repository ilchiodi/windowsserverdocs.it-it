---
ms.assetid: 0397c204-b3f8-4fd8-b71d-b7efb117766d
title: Fsutil volume
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: a8576dce4be639a516f8898e78bb6db12c91e171
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882452"
---
# <a name="fsutil-volume"></a>Fsutil volume
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Smonta un volume o una query il disco rigido per determinare la quantità di spazio libero è attualmente disponibile sul disco rigido o il file Usa un cluster specifico.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
fsutil volume [allocationreport] <VolumePath>
fsutil volume [diskfree] <VolumePath>
fsutil volume [dismount] <VolumePath>
fsutil volume [filelayout] <VolumePath> <fileid>
fsutil volume [list]
fsutil volume [querycluster] <VolumePath> <Cluster> [<Cluster>] … …
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|allocationreport|Visualizza le informazioni sull'uso di archiviazione in un determinato volume.|
|\<VolumePath>|Specifica la lettera di unità (seguita da due punti).|
|diskfree|Esegue una query il disco rigido per determinare la quantità di spazio disponibile su di esso.|
|smontare|Smonta un volume.|
|filelayout|Consente di visualizzare i metadati NTFS per il file specificato.|
|\<fileid>|Specifica l'id del file.|
|list|Elenca tutti i volumi del sistema.|
|querycluster|Trova il file utilizza un cluster specificato. È possibile specificare più cluster con il **querycluster** parametro.<br /><br />Questo parametro si applica a:  Windows Server 2008 R2 e Windows 7.|
|\<cluster>|Specifica il numero di cluster logico (LCN).|

## <a name="BKMK_examples"></a>Esempi
Per visualizzare un report i cluster allocati, digitare:

```
fsutil volume allocationreport C:
```

Per smontare un volume sull'unità C, digitare:

```
fsutil volume dismount c:
```

Per eseguire una query la quantità di spazio libero di un volume sull'unità C, digitare:

```
fsutil volume diskfree c:
```

Per visualizzare tutte le informazioni su un file specificati, digitare:

```
fsutil volume C: *
fsutil volume C:\Windows
fsutil volume C: 0x00040000000001bf
```

Per elencare i volumi sul disco, digitare:

```
fsutil volume list
```

Per trovare i file che utilizzano i cluster, specificati dai numeri cluster logico 50 e 0x2000, sull'unità C, digitare:

```
fsutil volume querycluster C: 50 0x2000
```

#### <a name="additional-references"></a>Altri riferimenti
[Chiave sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Funzionamento di NTFS](https://go.microsoft.com/fwlink/?LinkId=183396)


