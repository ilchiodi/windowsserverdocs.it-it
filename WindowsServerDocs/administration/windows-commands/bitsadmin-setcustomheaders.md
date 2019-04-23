---
title: setcustomheaders Bitsadmin
description: Argomento i comandi di Windows per **bitsadmin setcustomheaders** -aggiungere un'intestazione HTTP personalizzata a una richiesta GET.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d90ac2d23b852ae0c2114e7cd5a9c9e6382ce8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853852"
---
# <a name="bitsadmin-setcustomheaders"></a>setcustomheaders Bitsadmin



Aggiungere un'intestazione HTTP personalizzata a una richiesta GET.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetCustomHeaders <Job> <Header1> <Header2> <. . .>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Header2 Header1. . .|Le intestazioni personalizzate per il processo|

## <a name="remarks"></a>Note

-   Questa opzione consente di aggiungere un'intestazione HTTP personalizzata a una richiesta GET inviata a un server HTTP.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente aggiunge un'intestazione HTTP personalizzata per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin / SetCustomHeaders myDownloadJob "Accept-encoding:deflate/gzip"
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)