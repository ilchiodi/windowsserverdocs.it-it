---
title: list
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57b6c8d0-872e-4dba-9715-1db8ab892e98
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91b42925fc822b10157bb488167d06fe82cfe1e3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374705"
---
# <a name="list"></a>list



Elenca i writer, le copie shadow o provider di copie shadow attualmente registrato che il sistema. Se utilizzata senza parametri, **elenco** Visualizza la Guida al prompt dei comandi.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
list writers [metadata | detailed | status]
list shadows {all | set <SetID> | id <ShadowID>}
list providers
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|writer|Elenca i writer. Vedere [elenco writer](list-writers.md) per la sintassi e parametri.|
|ombreggiature|Elenca le copie shadow persistenti ed esistenti non persistenti. Vedere [elenco ombreggiature](list-shadows.md) per la sintassi e parametri.|
|provider|Elenchi attualmente registrati provider di copie shadow. Vedere [elenco provider](list-providers.md) per la sintassi e parametri.|

## <a name="BKMK_examples"></a>Esempi

Per elencare tutte le copie shadow, digitare:
```
list shadows all
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)