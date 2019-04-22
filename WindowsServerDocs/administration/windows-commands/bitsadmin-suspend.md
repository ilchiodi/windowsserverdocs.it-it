---
title: bitsadmin suspend
description: Argomento i comandi di Windows per **bitsadmin sospendere** -sospende il processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87e1bbd1b068d68fb60655043735c6c1aeb07707
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825922"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sospende il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Suspend <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Per riavviare il processo, usare il [Riprendi bitsadmin](bitsadmin-resume.md) passare.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente sospende il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /Suspend myDownloadJob
```

#### <a name="additional-references"></a>Riferimenti aggiuntivi

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
