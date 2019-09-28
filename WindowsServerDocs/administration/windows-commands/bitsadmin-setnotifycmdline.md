---
title: bitsadmin setnotifycmdline
description: Windows Commands Topic for * * * *-Bitsadmin setnotifycmdlineSets comando della riga di comando che viene eseguito al termine del processo di trasferimento dei dati o quando un processo entra in uno stato.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a307fe552e7d8ec5852de953a3a439cb02246ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380485"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Imposta il comando della riga di comando che viene eseguito al termine del processo di trasferimento dei dati o quando un processo entra in uno stato.

**BITS 1,2 e versioni precedenti**: Non supportati.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetNotifyCmdLine <Job> <ProgramName> [ProgramParameters]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|ProgramName|Nome del comando da eseguire al termine del processo.|
|ProgramParameters|I parametri che si desidera passare a *ProgramName*.|

## <a name="remarks"></a>Note

È possibile specificare NULL per *ProgramName* e *ProgramParameters*. Se *ProgramName* è NULL, *ProgramParameters* deve essere NULL.

> [!IMPORTANT]
> Se *ProgramParameters* non è NULL, quindi il primo parametro in *ProgramParameters* deve corrispondere *ProgramName*.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente imposta la riga di comando utilizzato dal servizio per eseguire il blocco note, quando il processo denominato *myDownloadJob* viene completata.
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe NULL
```
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe "notepad c:\eula.txt"
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)