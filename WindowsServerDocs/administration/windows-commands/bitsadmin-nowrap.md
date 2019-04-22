---
title: bitsadmin nowrap
description: Argomento i comandi di Windows per **nowrap bitsadmin** -tronca tutte le righe di output testo che si estende oltre il bordo all'estrema destra della finestra di comando.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4130f606a6b1874e1ea31952160de44d6e09c6b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822922"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

Tronca tutte le righe di output testo che si estende oltre il bordo all'estrema destra della finestra di comando.

## <a name="syntax"></a>Sintassi

```
bitsadmin /NoWrap
```

## <a name="remarks"></a>Note

Per impostazione predefinita, tutte le opzioni, ad eccezione di **Monitor** commutatore, eseguire il wrapping dell'output. Specificare il **NoWrap** passare prima di altre opzioni.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera lo stato del processo *myDownloadJob* e non va a capo l'output
```
C:\>bitsadmin /NoWrap /GetState myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)