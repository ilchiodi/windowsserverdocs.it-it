---
title: bitsadmin complete
description: Argomento i comandi di Windows per **bitsadmin completo** -completamento del processo. I file scaricati non sono disponibili fino a quando non si usa questa opzione.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 561585da370f7e69aa3b83b3ddd7579bfc658a21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817322"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

Completa il processo. I file scaricati non sono disponibili fino a quando non si usa questa opzione. Usare questa opzione dopo il processo passa allo stato trasferito. In caso contrario, solo i file che sono stati trasferiti sono disponibili.

## <a name="syntax"></a>Sintassi

```
bitsadmin /complete <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Quando lo stato del processo viene TRASFERITO, BITS avrà trasferito tutti i file del processo. Tuttavia, i file non sono disponibili finché non si usa la **/ completo** passare. Se più processi utilizzano *myDownloadJob* per il proprio nome, è necessario sostituire *myDownloadJob* con il GUID del processo per identificare in modo univoco il processo.
```
C:\>bitsadmin /complete myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)