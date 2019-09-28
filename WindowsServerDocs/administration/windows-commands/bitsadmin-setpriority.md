---
title: bitsadmin setpriority
description: 'Windows Commands Topic for **BITSAdmin sepriority** : imposta la priorità del processo specificato.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60564350928f917ca1861684e042304d5d380426
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380445"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority



Imposta la priorità del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetPriority <Job> <Priority>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Priority|Uno dei valori seguenti:</br>-IN PRIMO PIANO</br>-ALTO</br>-NORMALE</br>-BASSO|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente imposta la priorità per il processo denominato *myDownloadJob* Normal.
```
C:\>bitsadmin /SetPriority myDownloadJob NORMAL
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)