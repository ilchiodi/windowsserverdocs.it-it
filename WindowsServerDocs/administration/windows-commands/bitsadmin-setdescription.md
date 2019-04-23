---
title: bitsadmin setdescription
description: Argomento i comandi di Windows per **bitsadmin setdescription** -imposta la descrizione del processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e3323c20eebc8ba633ccfd478daa0753e506f46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830752"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription



Imposta la descrizione del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetDescription <Job> <Description>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Descrizione|Testo utilizzato per descrivere il processo.|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera la descrizione per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /SetDescription myDownloadJob "Music Downloads"
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)