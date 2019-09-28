---
title: Ripristinare
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3243f13a4997824d9fff7c874ce26d56325fefa4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371446"
---
# <a name="revert"></a>Ripristinare



Ripristina un volume in una copia shadow specificata. Questa è supportata solo per le copie shadow nel contesto CLIENTACCESSIBLE. Queste copie shadow sono persistenti e possono essere apportate solo dal provider di sistema. Se utilizzata senza parametri, **ripristinare** Visualizza la Guida al prompt dei comandi.

## <a name="syntax"></a>Sintassi

```
revert <ShadowCopyID>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<ShadowCopyID >|Specifica l'ID della copia shadow per ripristinare il volume.|

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)