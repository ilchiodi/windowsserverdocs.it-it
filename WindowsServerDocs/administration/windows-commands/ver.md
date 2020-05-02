---
title: ver
description: Argomento di riferimento per ver, che Visualizza il numero di versione del sistema operativo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7050dddda6cc27c50980f2e44f40e1f682c1d375
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720307"
---
# <a name="ver"></a>ver



Visualizza il numero di versione del sistema operativo.

Questo comando è supportato nel prompt dei comandi di Windows (cmd. exe), ma non in PowerShell.



## <a name="syntax"></a>Sintassi

```
ver
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="examples"></a>Esempi

Per ottenere il numero di versione del sistema operativo dalla shell dei comandi (cmd. exe), digitare:

```
ver
```

Il comando ver non funziona in PowerShell. Per ottenere la versione del sistema operativo da PowerShell, digitare:

```powershell
$PSVersionTable.BuildVersion
````


## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
