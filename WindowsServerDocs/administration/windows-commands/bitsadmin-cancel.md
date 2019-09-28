---
title: bitsadmin cancel
description: 'Argomento comandi di Windows per **BITSAdmin Annulla** : rimuove il processo dalla coda di trasferimento ed Elimina tutti i file temporanei associati al processo.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 77e46d787359af43a37faba5d844bfec09730454
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381799"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

Rimuove il processo dalla coda di trasferimento ed Elimina tutti i file temporanei associati al processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /cancel <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene rimosso il processo *myDownloadJob* dalla coda di trasferimento.
```
C:\>bitsadmin /cancel myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)