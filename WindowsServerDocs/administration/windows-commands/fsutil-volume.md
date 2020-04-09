---
ms.assetid: 0397c204-b3f8-4fd8-b71d-b7efb117766d
title: Fsutil volume
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 587ff48bd0af80667f9a336323641b87be808b1d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843934"
---
# <a name="fsutil-volume"></a>Fsutil volume
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Smonta un volume oppure esegue una query sull'unità disco rigido per determinare la quantità di spazio libero attualmente disponibile nell'unità disco rigido o il file che usa un cluster specifico.

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

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|allocationreport|Visualizza informazioni sull'utilizzo dell'archiviazione in un determinato volume.|
|\<VolumePath >|Specifica la lettera di unità (seguita da due punti).|
|DiskFree|Esegue una query sull'unità disco rigido per determinare la quantità di spazio disponibile.|
|smontare|Smonta un volume.|
|filelayout|Visualizza i metadati NTFS per il file specificato.|
|\<fileid >|Specifica l'ID del file.|
|list|Elenca tutti i volumi presenti nel sistema.|
|querycluster|Trova il file che usa un cluster specificato. È possibile specificare più cluster con il parametro **querycluster** .<p>Questo parametro si applica a: Windows Server 2008 R2 e Windows 7.|
|> \<cluster|Specifica il numero di cluster logico (LCN).|

## <a name="examples"></a><a name="BKMK_examples"></a>Esempi
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

## <a name="additional-references"></a>Altre informazioni di riferimento
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Fsutil](Fsutil.md)

[Funzionamento di NTFS](https://go.microsoft.com/fwlink/?LinkId=183396)


