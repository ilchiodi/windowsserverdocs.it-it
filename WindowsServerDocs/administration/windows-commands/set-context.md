---
title: Imposta contesto
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6f24e795f2d7c92d462cf822e70e4830b53827e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845852"
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
|Persistente|Specifica che la copia shadow viene mantenuta in uscita del programma, ripristino o il riavvio.|
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

[Chiave sintassi della riga di comando](command-line-syntax-key.md)