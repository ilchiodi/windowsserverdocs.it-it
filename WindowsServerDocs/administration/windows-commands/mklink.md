---
title: mklink
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: eabbf159a64ab5df7f45ece390d0c2fdb9956b80
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826332"
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
|/d|Crea un collegamento simbolico di directory. Per impostazione predefinita **mklink** crea un collegamento simbolico del file.|
|/h|Crea un collegamento fisso anziché un collegamento simbolico.|
|/j|Crea una giunzione Directory.|
|\<Collegamento >|Specifica il nome del collegamento simbolico che viene creato.|
|\<Target>|Specifica il percorso (relativo o assoluto) che si intende il nuovo collegamento simbolico.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_examples"></a>Esempi

Per creare un collegamento simbolico denominato MyDocs dalla directory radice della directory \Users\User1\Documents, digitare:
```
mklink /d \MyDocs \Users\User1\Documents
```
## <a name="additional-references"></a>Altri riferimenti
-   [New-Item](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
