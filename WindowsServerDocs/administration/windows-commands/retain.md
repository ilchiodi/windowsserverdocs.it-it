---
title: mantenere
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7391c0b60d07cc2eb5a6230b283ac180cc028508
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722345"
---
# <a name="retain"></a>mantenere



Prepara un volume semplice dinamico esistente da utilizzare come un avvio o il volume di sistema.

## <a name="syntax"></a>Sintassi

```
retain
```

## <a name="remarks"></a>Osservazioni

-   Su un disco dinamico MBR (record) di avvio principale, questo comando crea una voce di partizione nel record di avvio principale.
-   In un disco GPT (tabella) di partizione GUID, questo comando crea una voce di partizione della tabella di partizione GUID.

## <a name="additional-references"></a>Riferimenti aggiuntivi

