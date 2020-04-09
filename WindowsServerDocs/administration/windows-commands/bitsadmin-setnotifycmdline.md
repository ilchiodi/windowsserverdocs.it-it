---
title: bitsadmin setnotifycmdline
description: Windows Commands Topic for Bitsadmin setnotifycmdline, che consente di impostare il comando della riga di comando che viene eseguito al termine del processo di trasferimento dei dati o quando un processo entra in uno stato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 761a7003e44e8dc15cb2dd2f1ce5a1a23be53286
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849334"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Imposta il comando della riga di comando che viene eseguito al termine del processo di trasferimento dei dati o quando un processo entra in uno stato.

**BITS 1,2 e versioni precedenti**: non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetNotifyCmdLine <Job> <ProgramName> [ProgramParameters]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|ProgramName|Nome del comando da eseguire al termine del processo.|
|ProgramParameters|I parametri che si desidera passare a *ProgramName*.|

## <a name="remarks"></a>Note

È possibile specificare NULL per *ProgramName* e *ProgramParameters*. Se *ProgramName* è NULL, *ProgramParameters* deve essere NULL.

> [!IMPORTANT]
> Se *ProgramParameters* non è NULL, quindi il primo parametro in *ProgramParameters* deve corrispondere *ProgramName*.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente imposta la riga di comando utilizzato dal servizio per eseguire il blocco note, quando il processo denominato *myDownloadJob* viene completata.
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe NULL
```
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe notepad c:\eula.txt
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)