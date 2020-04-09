---
title: bitsadmin suspend
description: Argomento dei comandi di Windows per Bitsadmin Suspend, che sospende il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0419f4cdf59d04539b8b4c6d47cec886197d412b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849054"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sospende il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Suspend <Job>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Per riavviare il processo, usare l'opzione [BITSAdmin Resume](bitsadmin-resume.md) .

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene sospeso il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /Suspend myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
