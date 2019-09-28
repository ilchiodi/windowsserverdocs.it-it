---
ms.assetid: 77545920-2d13-4f35-a4d1-14dbec8340dc
title: Fsutil sparse
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: f9fb3cf46afb7e96c13fb623bc8f4fe67c1f3694
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376811"
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
|     queryflag     |                                                  Query sparse.                                                  |
|    queryrange     |                        Analizza un file e cerca gli intervalli che possono contenere dati diversi da zero.                        |
|      setflag      |                                        Contrassegna il file indicato come sparse.                                        |
|     SetRange      |                                   Riempie un intervallo specificato di un file con zeri.                                   |
|    <FileName>     | Specifica il percorso completo del file, inclusi il nome file e l'estensione, ad esempio C:\documents\filename.txt. |
| <BeginningOffset> |                              Specifica l'offset nel file da contrassegnare come sparse.                              |
|     <Length>      |                 Specifica la lunghezza dell'area nel file da contrassegnare come sparse (in byte).                 |

## <a name="remarks"></a>Note

-   Un file sparse è un file con una o più aree di dati non allocati. Un programma vedranno tali aree non allocate come contenente byte con valore zero, ma nessuno spazio su disco viene utilizzato per rappresentare questi zeri. Vengono allocati tutti i dati significativi o diversi da zero, mentre tutti i dati non significativi (stringhe di grandi dimensioni di dati costituiti da zeri) non vengono allocati. Quando viene letto un file sparse, i dati allocati vengono restituiti come archiviati e vengono restituiti dati non allocati, per impostazione predefinita, come zeri, in base alla specifica del requisito di sicurezza C2. Supporto file sparse consente di deallocare da ovunque nel file di dati.

-   In un file sparse, i grandi intervalli di zeri potrebbero non richiedere l'allocazione dei dischi. Lo spazio per i dati diversi da zero verrà allocato in base alle esigenze quando il file viene scritto.

-   Solo i file compressi o sparse possono avere intervalli azzerati noti al sistema operativo.

-   Se il file è di tipo sparse o compresso, NTFS potrebbe deallocare spazio su disco all'interno del file. Questo consente di impostare l'intervallo di byte su zero senza estendere le dimensioni del file.

## <a name="BKMK_examples"></a>Esempi
Per contrassegnare un file denominato Sample. txt nella directory C:\Temp come sparse, digitare:

```
fsutil sparse setflag c:\temp\sample.txt 
```

#### <a name="additional-references"></a>Altri riferimenti
[Indicazioni generali sulla sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


