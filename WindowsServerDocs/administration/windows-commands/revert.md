---
title: ripristinare
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5bc77b17317f602d642c7a9e025b67be10ad7256
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875112"
---
# <a name="revert"></a>ripristinare



Ripristina un volume in una copia shadow specificata. Questa è supportata solo per le copie shadow nel contesto CLIENTACCESSIBLE. Queste copie shadow sono persistenti e possono essere apportate solo dal provider di sistema. Se utilizzata senza parametri, **ripristinare** Visualizza la Guida al prompt dei comandi.

## <a name="syntax"></a>Sintassi

```
revert <ShadowCopyID>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<ShadowCopyID>|Specifica l'ID della copia shadow per ripristinare il volume.|

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)