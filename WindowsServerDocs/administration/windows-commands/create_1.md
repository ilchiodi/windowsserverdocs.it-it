---
title: creazione
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 837aa449-9b60-41ae-9ef1-ef67af6e5918
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2245efb6c3bce8aecf8edf730694804ffbdc3d80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378770"
---
# <a name="create"></a>creazione



Avvia il processo di creazione copia shadow, utilizzando le impostazioni correnti di contesto e l'opzione. Richiede almeno un volume in un Set di copia Shadow.

## <a name="syntax"></a>Sintassi

```
create
```

## <a name="remarks"></a>Note

-   È necessario aggiungere almeno un volume con il **aggiungere volume** comando prima di poter utilizzare il **creare** comando.
-   È possibile utilizzare il **iniziare backup** dei comandi per specificare un backup completo, anziché un backup di copia.
-   Dopo aver eseguito la **creare** è possibile utilizzare il comando di **exec** comando per eseguire uno script di duplicazione per il backup della copia shadow.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)