---
title: sethelpertoken Bitsadmin
description: Argomento i comandi di Windows per **bitsadmin sethelpertoken** -imposta token primario del prompt dei comandi corrente (o token di un account utente locali arbitrarie, se specificato) come token di supporto del processo di trasferimento BITS.
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
ms.openlocfilehash: 558a1aca66a7b3ec447136ceff9237d13efe4ede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853002"
---
# <a name="bitsadmin-sethelpertoken"></a>sethelpertoken Bitsadmin

Imposta token primario del prompt dei comandi corrente (o token di un account utente locali arbitrarie, se specificato) come un processo di trasferimento BITS [token helper](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs).

**BITS 3.0 e versioni precedenti**: Non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo.|
|\<username@domain\> \<password\>|Facoltativo&mdash;le credenziali di un utente locale account il cui token da usare.|

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
