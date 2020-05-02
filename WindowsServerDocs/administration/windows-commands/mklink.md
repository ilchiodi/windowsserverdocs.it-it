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
ms.openlocfilehash: 83a607711f0abe51810aa5abf4eb731206d810c2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723977"
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
## <a name="additional-references"></a>Altri riferimenti
-   [Nuovo elemento](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
-   [del](https://docs.microsoft.com/windows-server/administration/windows-commands/del)
-   [rmdir](https://docs.microsoft.com/windows-server/administration/windows-commands/rd)
