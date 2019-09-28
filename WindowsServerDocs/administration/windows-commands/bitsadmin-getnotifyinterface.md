---
title: bitsadmin getnotifyinterface
description: "Argomento dei comandi di Windows per **BITSAdmin getnotifyinterface** : determina se un altro programma ha registrato un'interfaccia di callback com per il processo specificato."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 826e13cf8a3e54935ceb5a72ff82647cacfc3be5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381471"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

Determina se un altro programma ha registrato un'interfaccia di callback COM (interfaccia Notify) per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetNotifyInterface <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Visualizza registrato o annullato.

> [!NOTE]
> Non è possibile determinare il programma che ha registrato l'interfaccia di callback.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene recuperata l'interfaccia Notify per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetNotifyInterface myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)