---
title: bitsadmin getminretrydelay
description: "Argomento dei comandi di Windows per **BITSAdmin getminretrydelay** : Recupera l'intervallo di tempo, in secondi, di attesa del servizio dopo aver rilevato un errore temporaneo prima di provare a trasferire il file."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a2bde6340034e48b97b4c86f48a3b2ef72560a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381547"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay



Recupera l'intervallo di tempo, in secondi, di attesa del servizio dopo che è stato rilevato un errore temporaneo prima di provare a trasferire il file.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetMinRetryDelay <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene recuperato il ritardo minimo tra tentativi per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetMinRetryDelay myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)