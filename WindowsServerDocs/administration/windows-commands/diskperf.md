---
title: diskperf
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 829f0284d761e6a5134011fa1dff99646d55fc13
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377807"
---
# <a name="diskperf"></a>diskperf



In Windows 2000, i contatori delle prestazioni dei dischi fisici e logici non sono abilitati per impostazione predefinita.

**Diskperf** è incluso in Windows XP, windows Server 2003, windows Server 2008, Windows Vista, windows Server 2008 R2 e Windows 7, in modo che possa essere utilizzato per abilitare o disabilitare in remoto i contatori delle prestazioni dei dischi fisici o logici nei computer che eseguono Windows 2000.

## <a name="syntax"></a>Sintassi

```
diskperf [-Y[D|V] | -N[D|V]] [\\computername]
```

## <a name="options"></a>Opzioni

|Opzione|Descrizione|
|------|-----------|
|-?|Vengono visualizzate sensibile al contesto della Guida.|
|-Y|Avviare tutti i contatori delle prestazioni del disco quando il computer viene riavviato.|
|-IARDE|Abilitare i contatori delle prestazioni del disco per le unità fisiche quando il computer viene riavviato.|
|-YV|Abilitare i contatori delle prestazioni del disco per le unità logiche o i volumi di archiviazione al riavvio del computer.|
|-N|Disabilitare tutti i contatori delle prestazioni del disco quando il computer viene riavviato.|
|-ND|Disabilitare i contatori delle prestazioni del disco per le unità fisiche quando il computer viene riavviato.|
|-NV|Disabilitare i contatori delle prestazioni del disco per le unità logiche o i volumi di archiviazione al riavvio del computer.|
|\\\\ *\<nomecomputer >*|Specificare il nome del computer in cui si desidera abilitare o disabilitare i contatori delle prestazioni del disco.|