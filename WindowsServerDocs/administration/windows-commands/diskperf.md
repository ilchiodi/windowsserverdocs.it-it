---
title: diskperf
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a99f18b56c9295e902a3c89e2e89b36c9c1b6c89
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852582"
---
# <a name="diskperf"></a>diskperf



In Windows 2000, i contatori delle prestazioni disco fisiche e logiche non sono abilitati per impostazione predefinita.

**Diskperf** è incluso in Windows XP, Windows Server 2003, Windows Server 2008, Windows Vista, Windows Server 2008 R2 e Windows 7 in modo che può essere utilizzato per abilitare o disabilitare i contatori delle prestazioni disco fisico o logico nei computer che eseguono in modalità remota Windows 2000.

## <a name="syntax"></a>Sintassi

```
diskperf [-Y[D|V] | -N[D|V]] [\\computername]
```

## <a name="options"></a>Opzioni

|Opzione|Descrizione|
|------|-----------|
|-?|Vengono visualizzate sensibile al contesto della Guida.|
|-Y|Avvia tutti i contatori delle prestazioni disco quando il computer viene riavviato.|
|-YD|Abilitare i contatori delle prestazioni del disco per le unità fisiche al riavvio del computer.|
|-YV|Abilitare i contatori delle prestazioni del disco per le unità logiche o i volumi di archiviazione al riavvio del computer.|
|-N|Disabilitare tutti i contatori delle prestazioni disco al riavvio del computer.|
|-ND|Disabilitare i contatori delle prestazioni del disco per le unità fisiche al riavvio del computer.|
|-NV|Disabilitare i contatori delle prestazioni del disco per le unità logiche o i volumi di archiviazione al riavvio del computer.|
|\\\\*\<computername>*|Specificare il nome del computer in cui si desidera abilitare o disabilitare i contatori delle prestazioni del disco.|