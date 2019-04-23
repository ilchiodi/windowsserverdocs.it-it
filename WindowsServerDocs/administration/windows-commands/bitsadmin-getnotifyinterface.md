---
title: bitsadmin getnotifyinterface
description: Argomento i comandi di Windows per **bitsadmin getnotifyinterface** -determina se un'interfaccia di callback COM per il processo specificato è registrato da un altro programma.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8316721a20cc477f9e8e15fc57b5d1c861da3ff4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868042"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

Determina se un altro programma è stata registrata un'interfaccia di callback di COM (l'interfaccia di notifica) per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetNotifyInterface <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Visualizza registrato o non registrato.

> [!NOTE]
> Non è possibile determinare il programma che ha registrato l'interfaccia di callback.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera l'interfaccia di notifica per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetNotifyInterface myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)