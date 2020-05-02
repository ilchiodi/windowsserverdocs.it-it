---
ms.assetid: 835fc6f1-cc84-4189-b29a-dde90792469e
title: Fsutil hardlink
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 860e843063db141c31cccdab5c3f5ffaa3a8aa7d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725499"
---
# <a name="fsutil-hardlink"></a>Fsutil hardlink
> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Crea un collegamento reale tra un file esistente e un nuovo file.

## <a name="syntax"></a>Sintassi

```
fsutil hardlink create <NewFileName> <ExistingFileName>
fsutil hardlink list <Filename>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|create|Stabilisce un collegamento fisso NTFS tra un file esistente e un nuovo file. Un collegamento hardware NTFS è simile a un collegamento reale POSIX.|
|\<> nomenuovofile|Specifica il file in cui si desidera creare un collegamento reale.|
|\<> ExistingFileName|Consente di specificare il file da cui si desidera creare un collegamento fisso.|
|list|Elenca il nome del *file*hardlinks.<p>Questo parametro si applica a: Windows Server 2008 R2 e Windows 7.|

## <a name="remarks"></a>Osservazioni

-   Un collegamento fisso è una voce di directory per un file. Ogni file può essere considerato disporre di almeno un collegamento fisso. Nei volumi NTFS ogni file può avere più collegamenti reali, quindi un singolo file può essere visualizzato in molte directory (o anche nella stessa directory con nomi diversi). Poiché tutti i collegamenti di riferimento nello stesso file, i programmi possono aprire i collegamenti e modificare il file. Un file viene eliminato dal file system solo dopo che tutti i collegamenti sono stati eliminati. Dopo aver creato un collegamento fisso, i programmi possono utilizzarlo come qualsiasi altro nome di file.

## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


