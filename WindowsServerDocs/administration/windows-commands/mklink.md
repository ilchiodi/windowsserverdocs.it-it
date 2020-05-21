---
title: mklink
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4bfa1c928b5bc5f4c5a885378f0f1d1c9b99cf5
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437136"
---
# <a name="mklink"></a>mklink
Crea un collegamento simbolico.



## <a name="syntax"></a>Sintassi

```
mklink [[/d] | [/h] | [/j]] <Link> <Target>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/d|Crea un collegamento simbolico di directory. Per impostazione predefinita, **mklink** crea un collegamento simbolico file.|
|/h|Crea un collegamento reale anziché un collegamento simbolico.|
|/j|Crea una giunzione di directory.|
|\<> di collegamento|Specifica il nome del collegamento simbolico da creare.|
|\<Target>|Specifica il percorso (relativo o assoluto) a cui fa riferimento il nuovo collegamento simbolico.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="examples"></a>Esempi

Per illustrare la creazione e la rimozione di un collegamento simbolico denominato fileFolder e MyFile. file dalla directory radice alla directory \Users\User1\Documents e un file di esempio. file che si trova all'interno della directory:
```
mklink /d \MyFolder \Users\User1\Documents
mklink /h \MyFile.file \User1\Documents\example.file
rd \MyFolder
del \MyFile.file
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Nuovo elemento](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
-   [del](https://docs.microsoft.com/windows-server/administration/windows-commands/del)
-   [rmdir](https://docs.microsoft.com/windows-server/administration/windows-commands/rd)
