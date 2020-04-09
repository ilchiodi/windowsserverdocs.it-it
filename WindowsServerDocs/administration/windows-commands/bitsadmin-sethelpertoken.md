---
title: sethelpertoken Bitsadmin
description: Windows Commands Topic for Bitsadmin sethelpertoken, che imposta il token primario del prompt dei comandi corrente (o un token dell'account utente locale arbitrario, se specificato) come token helper del processo di trasferimento BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: a1e8fd0054cadf3bf06b6e5b7bdf5010b18781e1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849534"
---
# <a name="bitsadmin-sethelpertoken"></a>sethelpertoken Bitsadmin

Imposta il token primario del prompt dei comandi corrente (o un token dell'account utente locale arbitrario, se specificato) come [token Helper](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)del processo di trasferimento BITS.

**BITS 3,0 e versioni precedenti**: non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o GUID del processo.|
|\<username@domain\> \<password\>|Facoltativo&mdash;le credenziali di un account utente locale il cui token deve essere utilizzato.|

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
