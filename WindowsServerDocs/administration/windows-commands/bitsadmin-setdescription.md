---
title: bitsadmin setdescription
description: Windows Commands Topic for **BITSAdmin sedescription** -imposta la descrizione del processo specificato.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d140ee9d575828a1a4d536073e468c9b4e56799f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380934"
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

Nell'esempio seguente viene recuperata la descrizione per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /SetDescription myDownloadJob "Music Downloads"
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)