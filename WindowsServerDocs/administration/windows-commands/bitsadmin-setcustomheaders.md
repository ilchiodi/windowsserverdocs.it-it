---
title: setcustomheaders Bitsadmin
description: Windows Commands Topic for Bitsadmin setcustomheaders, che aggiunge un'intestazione HTTP personalizzata a una richiesta GET.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5d97fae5f84637c80c3d1ef00aa36f09049bb17
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849614"
---
# <a name="bitsadmin-setcustomheaders"></a>setcustomheaders Bitsadmin

Aggiungere un'intestazione HTTP personalizzata a una richiesta GET.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetCustomHeaders <Job> <Header1> <Header2> <. . .>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Header2 Header1. . .|Le intestazioni personalizzate per il processo|

## <a name="remarks"></a>Note

-   Questa opzione consente di aggiungere un'intestazione HTTP personalizzata a una richiesta GET inviata a un server HTTP.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente aggiunge un'intestazione HTTP personalizzata per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin / SetCustomHeaders myDownloadJob Accept-encoding:deflate/gzip
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)