---
title: setcustomheaders Bitsadmin
description: Argomento dei comandi di Windows per **BITSAdmin setcustomheaders** -aggiungere un'intestazione HTTP personalizzata a una richiesta GET.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 45e3a5178df69b84618966ca0fcd9cc1e6d0e449
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380637"
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

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)