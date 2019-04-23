---
title: bitsadmin setdisplayname
description: Argomento i comandi di Windows per **bitsadmin setdisplayname** -imposta il nome visualizzato del processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d50cd2785e42b554cee340abc97fe4e4b53adcfc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843672"
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

[Chiave sintassi della riga di comando](command-line-syntax-key.md)