---
title: getpriority Bitsadmin
description: Argomento i comandi di Windows per **bitsadmin getpriority** -recupera la priorità del processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 6be2461ed87b75144367b1bd74376381e4674b66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841442"
---
# <a name="bitsadmin-getpriority"></a>getpriority Bitsadmin

Recupera la priorità del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetPriority <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

La priorità può essere **FOREGROUND**, **elevata**, **normale**, **bassa**, o **sconosciuto**.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera la priorità per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetPriority myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
