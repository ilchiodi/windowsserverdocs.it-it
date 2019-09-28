---
title: bitsadmin setdisplayname
description: Windows Commands Topic for **BITSAdmin sedisplayname** -imposta il nome visualizzato del processo specificato.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 10a5607eb26f8199ec415a4cec17d03015a26bcd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380628"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname



Imposta il nome visualizzato del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetDisplayName <Job> <DisplayName>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|DisplayName|Testo utilizzato per il nome visualizzato del processo specificato.|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente imposta il nome visualizzato per il processo denominato *myDownloadJob* a *myDownloadJob2*.
```
C:\>bitsadmin /SetDisplayName myDownloadJob "Download Music Job"
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)