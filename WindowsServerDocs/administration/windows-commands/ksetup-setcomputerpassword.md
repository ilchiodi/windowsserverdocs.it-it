---
title: ksetup:setcomputerpassword
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0679bb9ee429e05c7679411c5493bd21b530ef8e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831542"
---
# <a name="ksetupsetcomputerpassword"></a>ksetup:setcomputerpassword



Imposta la password per il computer locale. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /setcomputerpassword <Password>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Password>|Utilizza la password per impostare l'account computer del computer locale.</br>La password può essere impostata solo utilizzando un account con privilegi amministrativi. La password può contenere da 1 a 156 alfanumerico o di caratteri speciali.|

## <a name="remarks"></a>Note

Questo comando viene applicato solo l'account del computer.

È necessario riavviare il computer rendere effettiva la modifica di password.

La password dell'account computer non viene visualizzata nel Registro di sistema o come output il **che ksetup** comando.

## <a name="BKMK_Examples"></a>Esempi

Modificare la password dell'account computer del computer locale da IPops897 a IPop$ 897!.
```
ksetup /setcomputerpassword IPop$897!
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Ksetup](ksetup.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)