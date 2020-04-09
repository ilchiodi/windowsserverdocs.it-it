---
title: bitsadmin setdisplayname
description: Windows Commands Topic for Bitsadmin sedisplayname, che imposta il nome visualizzato del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 601c5b406132e70fb7d4facb97329f7456002bb4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849544"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname

Imposta il nome visualizzato del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetDisplayName <Job> <DisplayName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|DisplayName|Testo utilizzato per il nome visualizzato del processo specificato.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente imposta il nome visualizzato per il processo denominato *myDownloadJob* a *myDownloadJob2*.
```
C:\>bitsadmin /SetDisplayName myDownloadJob Download Music Job
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)