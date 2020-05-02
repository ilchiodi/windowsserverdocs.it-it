---
title: ripristinare
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b967904c28661be82909ebcc0aa7cb42c73e418c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722328"
---
# <a name="revert"></a>ripristinare



Ripristina un volume in una copia shadow specificata. Questa è supportata solo per le copie shadow nel contesto CLIENTACCESSIBLE. Queste copie shadow sono persistenti e possono essere apportate solo dal provider di sistema. Se utilizzata senza parametri, **ripristinare** Visualizza la Guida al prompt dei comandi.

## <a name="syntax"></a>Sintassi

```
revert <ShadowCopyID>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> ShadowCopyID|Specifica l'ID della copia shadow per ripristinare il volume.|

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)