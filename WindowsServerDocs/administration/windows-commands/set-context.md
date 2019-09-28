---
title: Imposta contesto
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 16f71d831f374f495abf2239cb8e694eee69efdf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370984"
---
# <a name="set-contex"></a>Set di menu di scelta rapida



Imposta il contesto per la creazione di copie shadow. Se utilizzata senza parametri, **Imposta contesto** Visualizza la Guida al prompt dei comandi.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|ClientAccessible|Specifica che la copia shadow è utilizzabile dalle versioni client di Windows.|
|persistente|Specifica che la copia shadow viene mantenuta in uscita del programma, ripristino o il riavvio.|
|volatile|Consente di eliminare l'ombreggiatura copiare sul uscire o reimpostare.|
|NoWriters|Specifica che tutti i writer sono esclusi.|

## <a name="remarks"></a>Note

-   Il *clientaccessible* contesto è persistente per impostazione predefinita.

## <a name="BKMK_examples"></a>Esempi

Per impedire che le copie shadow viene eliminata quando si esce dalla DiskShadow, digitare:
```
set context persistent
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)