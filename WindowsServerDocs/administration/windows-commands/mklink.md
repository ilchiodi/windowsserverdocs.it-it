---
title: mklink
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d930cbf7acbfceab16f2fa619aaaac6e789c131
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373637"
---
# <a name="mklink"></a>mklink
Crea un collegamento simbolico.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
mklink [[/d] | [/h] | [/j]] <Link> <Target>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/d|Crea un collegamento simbolico di directory. Per impostazione predefinita, **mklink** crea un collegamento simbolico file.|
|/h|Crea un collegamento reale anziché un collegamento simbolico.|
|/j|Crea una giunzione di directory.|
|\<Link >|Specifica il nome del collegamento simbolico da creare.|
|\<Target >|Specifica il percorso (relativo o assoluto) a cui fa riferimento il nuovo collegamento simbolico.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_examples"></a>Esempi

L'esempio follwing illustra la creazione e la rimozione di un collegamento simbolico denominato MyFile e MyFile. file dalla directory radice alla directory \Users\User1\Documents e un file di esempio. file che si trova nella directory:
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
