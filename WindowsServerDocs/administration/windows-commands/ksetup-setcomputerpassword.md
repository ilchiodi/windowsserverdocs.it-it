---
title: 'che Ksetup: setcomputerpassword'
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e65ea6e935d9fde9c23842755c36e418928dec7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841374"
---
# <a name="ksetupsetcomputerpassword"></a>che Ksetup: setcomputerpassword



Imposta la password per il computer locale. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /setcomputerpassword <Password>
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|> password \<|Utilizza la password per impostare l'account computer del computer locale.</br>La password può essere impostata solo utilizzando un account con privilegi amministrativi. La password può contenere da 1 a 156 alfanumerico o di caratteri speciali.|

## <a name="remarks"></a>Note

Questo comando viene applicato solo l'account del computer.

È necessario riavviare il computer rendere effettiva la modifica di password.

La password dell'account computer non viene visualizzata nel Registro di sistema o come output il **che ksetup** comando.

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

Modificare la password dell'account computer del computer locale da IPops897 a IPop$ 897!.
```
ksetup /setcomputerpassword IPop$897!
```

## <a name="additional-references"></a>Altre informazioni di riferimento

-   [Che Ksetup](ksetup.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)