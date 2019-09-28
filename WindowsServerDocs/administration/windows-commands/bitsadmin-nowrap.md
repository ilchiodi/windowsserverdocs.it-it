---
title: bitsadmin nowrap
description: "Argomento dei comandi di Windows per **BITSAdmin nowrap** : tronca qualsiasi riga di testo di output che si estende oltre il bordo all'estrema destra della finestra di comando."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3806ec51161eeae498e3c9b367b2aacf0bd32c99
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381048"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

Tronca tutte le righe di output testo che si estende oltre il bordo all'estrema destra della finestra di comando.

## <a name="syntax"></a>Sintassi

```
bitsadmin /NoWrap
```

## <a name="remarks"></a>Note

Per impostazione predefinita, tutte le opzioni, ad eccezione dell'opzione di **monitoraggio** , incapsulano l'output. Specificare l'opzione **nowrap** prima di altre opzioni.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera lo stato del processo *myDownloadJob* e non va a capo l'output
```
C:\>bitsadmin /NoWrap /GetState myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)