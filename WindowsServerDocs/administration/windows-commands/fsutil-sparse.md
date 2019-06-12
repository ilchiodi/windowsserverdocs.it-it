---
ms.assetid: 77545920-2d13-4f35-a4d1-14dbec8340dc
title: Fsutil sparse
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: b1bc4e45ed2a2b06c72318e0999988ed8f016c40
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438975"
---
# <a name="fsutil-sparse"></a>Fsutil sparse
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gestisce i file sparse.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
fsutil sparse [queryflag] <FileName>
fsutil sparse [queryrange] <FileName>
fsutil sparse [setflag] <FileName>
fsutil sparse [setrange] <FileName> <BeginningOffset> <Length>
```

## <a name="parameters"></a>Parametri

|     Parametro     |                                                    Descrizione                                                    |
|-------------------|-------------------------------------------------------------------------------------------------------------------|
|     queryflag     |                                                  Query di tipo sparse.                                                  |
|    queryrange     |                        Cerca un file e ricerca gli intervalli che possono contenere dati diverso da zero.                        |
|      setflag      |                                        Contrassegna il file indicato come sparse.                                        |
|     SetRange      |                                   Riempie un intervallo specificato di un file con zeri.                                   |
|    <FileName>     | Specifica il percorso completo del file incluso il nome del file e l'estensione, ad esempio C:\documents\filename.txt. |
| <BeginningOffset> |                              Specifica l'offset all'interno del file da contrassegnare come di tipo sparse.                              |
|     <Length>      |                 Specifica la lunghezza dell'area del file siano contrassegnati come sparse (in byte).                 |

## <a name="remarks"></a>Note

-   Un file sparse è un file con una o più aree di dati non allocati. Un programma vedranno tali aree non allocate come contenente byte con valore zero, ma nessuno spazio su disco viene utilizzato per rappresentare questi zeri. Tutti i dati significativi o diverso da zero è allocato, mentre tutti i dati nonmeaningful (stringhe di grandi dimensioni di dati che sono costituiti da zeri) non è allocata. Quando viene letto un file sparse, dati allocati vengono restituiti come archiviati e per impostazione predefinita, i dati non allocati vengono restituiti come zeri, conforme alla specifica di requisiti di sicurezza C2. Supporto file sparse consente di deallocare da ovunque nel file di dati.

-   In un file sparse, grandi intervalli di zeri non richiedano l'allocazione dei dischi. In base alle necessità quando viene scritto il file verrà allocato spazio per i dati di diverso da zero.

-   Solo i file di tipo sparse o compressi possono avere zeroed intervalli noti al sistema operativo.

-   Se il file è di tipo sparse o compressi, NTFS può deallocare spazio su disco all'interno del file. Questo imposta l'intervallo di byte su zero senza aumentare le dimensioni del file.

## <a name="BKMK_examples"></a>Esempi
Per contrassegnare un file denominato Sample. txt nella directory C:\Temp di tipo sparse, digitare:

```
fsutil sparse setflag c:\temp\sample.txt 
```

#### <a name="additional-references"></a>Altri riferimenti
[Indicazioni generali sulla sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


