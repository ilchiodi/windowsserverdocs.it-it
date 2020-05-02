---
title: 'che Ksetup: setcomputerpassword'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9cb0c2ee36ed85ddfb015a80e86198fe788f8474
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724587"
---
# <a name="ksetupsetcomputerpassword"></a>che Ksetup: setcomputerpassword



Imposta la password per il computer locale.

## <a name="syntax"></a>Sintassi

```
ksetup /setcomputerpassword <Password>
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> password|Utilizza la password per impostare l'account computer del computer locale.</br>La password può essere impostata solo utilizzando un account con privilegi amministrativi. La password può contenere da 1 a 156 alfanumerico o di caratteri speciali.|

## <a name="remarks"></a>Osservazioni

Questo comando viene applicato solo l'account del computer.

È necessario riavviare il computer rendere effettiva la modifica di password.

La password dell'account computer non viene visualizzata nel Registro di sistema o come output il **che ksetup** comando.

## <a name="examples"></a>Esempi

Modificare la password dell'account computer del computer locale da IPops897 a IPop$ 897!.
```
ksetup /setcomputerpassword IPop$897!
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Che Ksetup](ksetup.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)