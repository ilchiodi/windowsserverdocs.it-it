---
title: clean
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd5eb2ec1bde4523eb6f0f919e09b9711b2654fb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434325"
---
# <a name="clean"></a>clean

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il comando di Diskpart pulito rimuove tutte le partizioni o volumi formattati da un disco con lo stato attivo.
## <a name="syntax"></a>Sintassi
```
clean [all]
```
## <a name="parameters"></a>Parametri

| Parametro |                                                        Descrizione                                                        |
|-----------|---------------------------------------------------------------------------------------------------------------------------|
|    tutti    | Specifica che ogni settore del disco è impostato su zero, che elimina completamente tutti i dati contenuti nel disco. |

## <a name="remarks"></a>Note
- Nei dischi di (avvio principale MBR) master boot record, solo il partizionamento MBR informazioni e settori nascosti vengono sovrascritte.
- Nei dischi tabella di partizione GUID (gpt), le informazioni, incluso il settore MBR, al partizionamento gpt viene sovrascritto. Non sono disponibili informazioni di settori nascosti.
- Per eseguire questa operazione, è necessario selezionare un disco. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.
  ## <a name="BKMK_examples"></a>Esempi
  Per rimuovere tutta la formattazione del disco selezionato, digitare:
  ```
  clean
  ```

[Clear-Disk](https://technet.microsoft.com/library/hh848661.aspx)
