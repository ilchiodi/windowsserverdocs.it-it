---
title: Imposta contesto
description: Windows Commands argomento per set context, che imposta il contesto per la creazione di copie shadow.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 162fefc46bc3b8ae39dcb41a387e50ed98fefa70
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834564"
---
# <a name="set-contex"></a>Set di menu di scelta rapida

Imposta il contesto per la creazione di copie shadow. Se utilizzata senza parametri, **Imposta contesto** Visualizza la Guida al prompt dei comandi.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|ClientAccessible|Specifica che la copia shadow è utilizzabile dalle versioni client di Windows.|
|persistente|Specifica che la copia shadow viene mantenuta in uscita del programma, ripristino o il riavvio.|
|volatile|Consente di eliminare l'ombreggiatura copiare sul uscire o reimpostare.|
|NoWriters|Specifica che tutti i writer sono esclusi.|

## <a name="remarks"></a>Note

-   Il *clientaccessible* contesto è persistente per impostazione predefinita.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per impedire che le copie shadow viene eliminata quando si esce dalla DiskShadow, digitare:
```
set context persistent
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)