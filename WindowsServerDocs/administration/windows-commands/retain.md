---
title: Conservare
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4b437e9f0c8d671e4378311d450aa0ac7639219f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852172"
---
# <a name="retain"></a>Conservare



Prepara un volume semplice dinamico esistente da utilizzare come un avvio o il volume di sistema.

## <a name="syntax"></a>Sintassi

```
retain
```

## <a name="remarks"></a>Note

-   Su un disco dinamico MBR (record) di avvio principale, questo comando crea una voce di partizione nel record di avvio principale.
-   In un disco GPT (tabella) di partizione GUID, questo comando crea una voce di partizione della tabella di partizione GUID.

#### <a name="additional-references"></a>Altri riferimenti

