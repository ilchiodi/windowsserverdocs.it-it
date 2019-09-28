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
ms.openlocfilehash: de9a04850555b11a42d74826685c249bea15710c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376882"
---
# <a name="fsutil-hardlink"></a>Fsutil hardlink
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Crea un collegamento reale tra un file esistente e un nuovo file.

## <a name="syntax"></a>Sintassi

```
fsutil hardlink create <NewFileName> <ExistingFileName>
fsutil hardlink list <Filename>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|creazione|Stabilisce un collegamento fisso NTFS tra un file esistente e un nuovo file. Un collegamento hardware NTFS è simile a un collegamento reale POSIX.|
|\<NewFileName >|Specifica il file in cui si desidera creare un collegamento reale.|
|\<ExistingFileName >|Consente di specificare il file da cui si desidera creare un collegamento fisso.|
|list|Elenca il nome del *file*hardlinks.<br /><br />Questo parametro si applica a:  Windows Server 2008 R2 e Windows 7.|

## <a name="remarks"></a>Note

-   Un collegamento fisso è una voce di directory per un file. Ogni file può essere considerato disporre di almeno un collegamento fisso. Nei volumi NTFS ogni file può avere più collegamenti reali, quindi un singolo file può essere visualizzato in molte directory (o anche nella stessa directory con nomi diversi). Poiché tutti i collegamenti di riferimento nello stesso file, i programmi possono aprire i collegamenti e modificare il file. Un file viene eliminato dal file system solo dopo che tutti i collegamenti sono stati eliminati. Dopo aver creato un collegamento fisso, i programmi possono utilizzarlo come qualsiasi altro nome di file.

#### <a name="additional-references"></a>Altri riferimenti
[Indicazioni generali sulla sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


