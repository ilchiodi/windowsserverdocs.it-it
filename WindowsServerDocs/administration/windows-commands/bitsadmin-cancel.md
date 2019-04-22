---
title: bitsadmin cancel
description: 'Argomento i comandi di Windows per **bitsadmin Annulla** : rimuove il processo dalla coda di trasferimento ed Elimina tutti i file temporanei associati al processo.'
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0a4d1e2d6e4fd66cb525316f236d070fcd72d73f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814072"
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

L'esempio seguente rimuove il *myDownloadJob* processo dalla coda di trasferimento.
```
C:\>bitsadmin /cancel myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)