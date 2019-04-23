---
title: ver
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 384a5e8adb6c8304033f7dc645184ff2b674ae39
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887172"
---
# <a name="ver"></a>ver



Visualizza il numero di versione del sistema operativo.

Questo comando è supportato nel prompt dei comandi di Windows (Cmd.exe), ma non in PowerShell.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
ver
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_examples"></a>Esempi

Per ottenere il numero di versione del sistema operativo dalla shell dei comandi (cmd.exe), digitare:

```
ver
```

Il comando ver non funziona in PowerShell. Per ottenere la versione del sistema operativo di PowerShell, digitare:

```powershell
$PSVersionTable.BuildVersion
````


#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
