---
title: mantenere
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b076e12c833645833f53a06476e62bbf44f2690
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384487"
---
# <a name="retain"></a>mantenere



Prepara un volume semplice dinamico esistente da utilizzare come un avvio o il volume di sistema.

## <a name="syntax"></a>Sintassi

```
retain
```

## <a name="remarks"></a>Note

-   Su un disco dinamico MBR (record) di avvio principale, questo comando crea una voce di partizione nel record di avvio principale.
-   In un disco GPT (tabella) di partizione GUID, questo comando crea una voce di partizione della tabella di partizione GUID.

#### <a name="additional-references"></a>Altri riferimenti

