---
title: create
description: Argomento di riferimento per create, che avvia il processo di creazione della copia shadow, usando il contesto e le impostazioni delle opzioni correnti.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 837aa449-9b60-41ae-9ef1-ef67af6e5918
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cfddbebd5744d8cd222d67e46690ce8b5d2e0fde
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716841"
---
# <a name="create"></a>create

Avvia il processo di creazione copia shadow, utilizzando le impostazioni correnti di contesto e l'opzione. Richiede almeno un volume in un Set di copia Shadow.

## <a name="syntax"></a>Sintassi

```
create
```

## <a name="remarks"></a>Osservazioni

-   È necessario aggiungere almeno un volume con il **aggiungere volume** comando prima di poter utilizzare il **creare** comando.
-   È possibile utilizzare il **iniziare backup** dei comandi per specificare un backup completo, anziché un backup di copia.
-   Dopo aver eseguito la **creare** è possibile utilizzare il comando di **exec** comando per eseguire uno script di duplicazione per il backup della copia shadow.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)