---
title: bitsadmin takeownership
description: Windows Commands Topic for Bitsadmin TakeOwnership, che consente a un utente con privilegi amministrativi di assumere la proprietà del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a2c0bfc1fcb1606102aece76129c49aad701ead
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849024"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership

Consente a un utente con privilegi amministrativi di assumere la proprietà del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /TakeOwnership <Job>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

L'esempio seguente acquisisce la proprietà del processo denominato *myDownloadJob*.
```
C:\>bitsadmin /TakeOwnership myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)