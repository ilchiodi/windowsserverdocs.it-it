---
title: ver
description: Windows Commands Topic for ver, che Visualizza il numero di versione del sistema operativo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0d0676dcfa6546e4bbf74c4c58a24f51744d00f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830194"
---
# <a name="ver"></a>ver



Visualizza il numero di versione del sistema operativo.

Questo comando è supportato nel prompt dei comandi di Windows (cmd. exe), ma non in PowerShell.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
ver
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per ottenere il numero di versione del sistema operativo dalla shell dei comandi (cmd. exe), digitare:

```
ver
```

Il comando ver non funziona in PowerShell. Per ottenere la versione del sistema operativo da PowerShell, digitare:

```powershell
$PSVersionTable.BuildVersion
````


## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
