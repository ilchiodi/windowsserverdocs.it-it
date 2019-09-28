---
title: bitsadmin takeownership
description: Argomento dei comandi di Windows per **BITSAdmin TakeOwnership** -consente a un utente con privilegi amministrativi di assumere la proprietà del processo specificato.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f0d0610b2ba6437f6fdd41bf1b875993cf11f2a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380364"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership



Consente a un utente con privilegi amministrativi di assumere la proprietà del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /TakeOwnership <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

L'esempio seguente acquisisce la proprietà del processo denominato *myDownloadJob*.
```
C:\>bitsadmin /TakeOwnership myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)