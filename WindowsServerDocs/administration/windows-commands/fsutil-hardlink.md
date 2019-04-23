---
ms.assetid: 835fc6f1-cc84-4189-b29a-dde90792469e
title: Fsutil hardlink
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 69474bd1817471176598afba508cd80c8fa1df8a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840052"
---
# <a name="fsutil-hardlink"></a>Fsutil hardlink
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Crea un collegamento tra un file esistente e un nuovo file.

## <a name="syntax"></a>Sintassi

```
fsutil hardlink create <NewFileName> <ExistingFileName>
fsutil hardlink list <Filename>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|creazione|Stabilisce un collegamento di disco rigido NTFS tra un file esistente e un nuovo file. (Un collegamento fisso NTFS è simile a un collegamento reale POSIX).|
|\<NewFileName>|Specifica il file che si desidera creare un collegamento a.|
|\<ExistingFileName>|Specifica il file che si desidera creare un collegamento da.|
|list|Elenca le hardlinks al *Filename*.<br /><br />Questo parametro si applica a:  Windows Server 2008 R2 e Windows 7.|

## <a name="remarks"></a>Note

-   Un collegamento reale è una voce di directory per un file. Ogni file può essere considerato disporre di almeno un collegamento fisso. Nei volumi NTFS, i file possono avere più collegamenti fissi, pertanto un singolo file possono essere visualizzati in diverse directory (o persino nella stessa directory con nomi diversi). Poiché tutti i collegamenti di riferimento nello stesso file, i programmi possono aprire i collegamenti e modificare il file. Un file viene eliminato dal file system solo dopo l'eliminazione di tutti i collegamenti a esso. Dopo aver creato un collegamento fisso, i programmi possono utilizzarlo come qualsiasi altro nome di file.

#### <a name="additional-references"></a>Altri riferimenti
[Chiave sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


