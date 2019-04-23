---
title: Utilizzando il comando get-DriverPackageFile
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed9518fae07745502d01dc0084b7443a1332db83
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859802"
---
# <a name="using-the-get-driverpackagefile-command"></a>Utilizzando il comando get-DriverPackageFile



Visualizza informazioni su un pacchetto driver, inclusi i driver e file in che esso contenuti.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/ InfFile:\<percorso del Inf File >|Specifica il nome di file e percorso completo del file di pacchetto di driver inf.|
|[/Architecture:{x86 | ia64 | x64}]|Specifica l'architettura del pacchetto driver.|
|[/Show: {driver | File | All}]|Indica le informazioni sul pacchetto da visualizzare. Se **/Mostra** non viene specificato, il valore predefinito è per restituire solo i driver dei metadati del pacchetto. **I driver** Visualizza l'elenco dei driver nel pacchetto. **File** consente di visualizzare l'elenco dei file nel pacchetto. **Tutti** Visualizza driver e file.|

## <a name="BKMK_examples"></a>Esempi

Per visualizzare informazioni su un file di driver, digitare:
```
WDSUTIL /Get-DriverPackageFile /InfFile:"C:\temp\1394.inf" /Architecture:x86
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)