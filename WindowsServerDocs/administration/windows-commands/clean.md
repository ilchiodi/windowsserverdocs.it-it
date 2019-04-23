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
ms.openlocfilehash: 6e7d7613784f3e599005b25259f70466db60e626
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877432"
---
# <a name="clean"></a>clean

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il comando di Diskpart pulito rimuove tutte le partizioni o volumi formattati da un disco con lo stato attivo.
## <a name="syntax"></a>Sintassi
```
clean [all]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|tutti|Specifica che ogni settore del disco è impostato su zero, che elimina completamente tutti i dati contenuti nel disco.|
## <a name="remarks"></a>Note
-   Nei dischi di (avvio principale MBR) master boot record, solo il partizionamento MBR informazioni e settori nascosti vengono sovrascritte.
-   Nei dischi tabella di partizione GUID (gpt), le informazioni, incluso il settore MBR, al partizionamento gpt viene sovrascritto. Non sono disponibili informazioni di settori nascosti.
-   Per eseguire questa operazione, è necessario selezionare un disco. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.
## <a name="BKMK_examples"></a>Esempi
Per rimuovere tutta la formattazione del disco selezionato, digitare:
```
clean
```

[Clear-Disk](https://technet.microsoft.com/library/hh848661.aspx)
