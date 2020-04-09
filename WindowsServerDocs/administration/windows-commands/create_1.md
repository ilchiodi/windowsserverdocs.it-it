---
title: creazione
description: Windows Commands Topic for create, che avvia il processo di creazione della copia shadow, usando il contesto e le impostazioni delle opzioni correnti.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 837aa449-9b60-41ae-9ef1-ef67af6e5918
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d29285517ca678a15828079c95663fc4d501eaf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846834"
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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)