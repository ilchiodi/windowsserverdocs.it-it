---
title: list
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57b6c8d0-872e-4dba-9715-1db8ab892e98
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d60c42b868a1e9a26e3168e4b489573f9f87e179
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841104"
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

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|writer|Elenca i writer. Vedere [elenco writer](list-writers.md) per la sintassi e parametri.|
|ombreggiature|Elenca le copie shadow persistenti ed esistenti non persistenti. Vedere [elenco ombreggiature](list-shadows.md) per la sintassi e parametri.|
|provider|Elenchi attualmente registrati provider di copie shadow. Vedere [elenco provider](list-providers.md) per la sintassi e parametri.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per elencare tutte le copie shadow, digitare:
```
list shadows all
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)