---
title: bitsadmin setnotifycmdline
description: Argomento i comandi di Windows per * * *-bitsadmin setnotifycmdlineSets la riga di comando che verrà eseguito quando il processo viene completato il trasferimento dei dati o quando un processo entra in uno stato.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f1cea4e99cbaaf3881c6f436bdb932090ad6b006
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859072"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Imposta la riga di comando che verrà eseguito quando il processo viene completato il trasferimento dei dati o quando un processo entra in uno stato.

**BITS 1.2 e versioni precedenti**: Non supportato.

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

[Chiave sintassi della riga di comando](command-line-syntax-key.md)