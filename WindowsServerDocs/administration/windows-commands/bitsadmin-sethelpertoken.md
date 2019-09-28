---
title: sethelpertoken Bitsadmin
description: "Argomento dei comandi di Windows per **BITSAdmin sethelpertoken** : imposta il token primario del prompt dei comandi corrente (o un token dell'account utente locale arbitrario, se specificato) come token helper del processo di trasferimento BITS."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 91c03366998168dad9ab4530ef36a5020b8ad6ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380575"
---
# <a name="bitsadmin-sethelpertoken"></a>sethelpertoken Bitsadmin

Imposta il token primario del prompt dei comandi corrente (o un token dell'account utente locale arbitrario, se specificato) come [token Helper](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)del processo di trasferimento BITS.

**BITS 3,0 e versioni precedenti**: Non supportati.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o GUID del processo.|
|\< @ no__t-1 @ no__t-2 \<password @ no__t-4|Facoltativo @ no__t-0The le credenziali di un account utente locale il cui token deve essere utilizzato.|

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
